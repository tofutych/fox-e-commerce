<template>
  <div id="wrapper">
    <nav class="navbar">
      <div class="navbar-brand">
        <router-link to="/" class="navbar-item"><strong>Foxxx</strong>
          <img src="https://img.icons8.com/color/96/000000/fox.png"/>
        </router-link>
        <a class="navbar-burger" aria-label="menu" aria-expanded="false" data-target="navbar-menu"
           @click="showMobileMenu = !showMobileMenu">
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
        </a>
      </div>

      <div class="navbar-menu" id="navbar-menu" v-bind:class="{'is-active': showMobileMenu}">
        <div class="navbar-start">

          <router-link to="/bastard" class="navbar-item">Bastard</router-link>
          <router-link to="/red" class="navbar-item">Red</router-link>
          <div class="navbar-item">
            <form method="get" action="/search">
              <div class="field has-addons">
                <div class="control">
                  <input type="text" class="input is-warning" placeholder="Search here..." name="query">
                </div>

                <div class="control">
                  <button class="button is-warning">
                    <span class="icon">
                      <i class="fas fa-search"></i>
                    </span>
                  </button>
                </div>
              </div>
            </form>
          </div>
        </div>

        <div class="navbar-end">
          <div class="navbar-item">
            <div class="buttons">
              <router-link to="/cart" class="button is-warning">
                <span class="icon"><i class="fas fa-shopping-cart"></i></span>
                <span>Cart ({{ cartTotalLength }})</span>
              </router-link>
              <router-link to="/log-in" class="button is-light">Log in</router-link>

            </div>
          </div>
        </div>
      </div>
    </nav>

    <div class="is-loading-bar has-text-centered" v-bind:class="{ 'is-loading': $store.state.isLoading }">
      <div class="lds-dual-ring"></div>
    </div>

    <section class="section">
      <router-view/>
    </section>

    <footer class="footer has-background-white-bis">
      <div class="content has-text-centered">
        <p>Foxyright (c) 2022</p>
      </div>
    </footer>
  </div>
</template>

<script>
export default {
  data() {
    return {
      showMobileMenu: false,
      cart: {
        items: []
      }
    }
  },

  beforeCreate() {
    this.$store.commit('initializeStore')
  },

  mounted() {
    this.cart = this.$store.state.cart
  },

  computed: {
    cartTotalLength() {
      let totalLength = 0

      for (let i = 0; i < this.cart.items.length; i++) {
        totalLength += this.cart.items[i].quantity
      }

      return totalLength
    }
  }
}
</script>

<style lang="scss">
@import "bulma";

.lds-dual-ring {
  display: inline-block;
  opacity: 69%;
  width: 40px;
  height: 40px;
}

.lds-dual-ring:after {
  content: " ";
  display: block;
  width: 32px;
  height: 32px;
  margin: 4px;
  border-radius: 50%;
  border: 6px solid #ccc;
  border-color: hsl(44deg, 100%, 77%) transparent hsl(44deg, 100%, 77%) transparent;
  animation: lds-dual-ring 1.2s linear infinite;
}

@keyframes lds-dual-ring {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

.is-loading-bar {
  height: 0;
  overflow: hidden;
  -webkit-transition: all 0.3s;
  transition: all 0.3s;

  &.is-loading {
    height: 40px;
  }
}

$navbar-item-color: lighten($text, 30%);
</style>

<style lang="css">
.router-link-exact-active.navbar-item {
  -webkit-animation: 0.3s highlight 0.3s 1 normal forwards;
  animation: 0.3s highlight 0.3s 1 normal forwards;
  background-image: linear-gradient(90deg, hsl(44deg, 100%, 77%) 50%, rgba(255, 255, 255, 0) 50%);
  background-size: 200% 100%;
  background-position: 100% 0;
}

@-webkit-keyframes highlight {
  to {
    background-position: 0 0;
  }
}

@keyframes highlight {
  to {
    background-position: 0 0;
  }
}

</style>