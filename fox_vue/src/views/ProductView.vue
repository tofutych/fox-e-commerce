<template>
  <section class="section">
    <div class="container">
      <div class="columns is-vcentered is-multiline">
        <div class="column is-6-tablet is-3-desktop">
          <h1 class="is-size-3-mobile is-size-1-desktop title">{{ product.name }}</h1>
          <p>{{ product.description }}</p>
        </div>
        <div class="column is-6-tablet is-5-desktop has-text-centered">
          <img v-bind:src="product.get_image" :alt="product.name">
        </div>
        <div class="column is-12-tablet is-4-desktop">
          <div class="is-size-4 mb-4">${{ product.price }}</div>
          <form>

            <div class="field has-addons mt-6">
              <div class="control">
                <input type="number" class="input is-primary is-large" min="1" v-model="quantity">
              </div>

              <div class="control">
                <a class="button is-primary is-large is-outlined" @click="addToCart">Add to cart</a>
              </div>
            </div>

          </form>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import axios from "axios"
import {toast} from "bulma-toast"

export default {
  name: "ProductView.vue",
  data() {
    return {
      product: {},
      quantity: 1
    }
  },
  mounted() {
    this.getProduct()
  },
  methods: {
    async getProduct() {
      this.$store.commit('setIsLoading', true)

      const category_slug = this.$route.params.category_slug
      const product_slug = this.$route.params.product_slug

      await axios
          .get(`/api/v1/products/${category_slug}/${product_slug}`)
          .then(response => {
            this.product = response.data

            document.title = this.product.name + ' | Foxes'
          })
          .catch(error => {
            console.log(error)
          })

      this.$store.commit('setIsLoading', false)
    },

    addToCart() {
      if (isNaN(this.quantity) || this.quantity < 1) {
        this.quantity = 1
      }

      const item = {
        product: this.product,
        quantity: this.quantity
      }

      this.$store.commit('addToCart', item)

      toast({
        message: `«${this.product.name}» was added to the cart`,
        type: 'is-success',
        dismissible: false,
        pauseOnHover: true,
        duration: 2000,
        position: 'bottom-right',
        animate: {
          in: 'fadeIn',
          out: 'fadeOut'
        }
      })
    }
  }
}
</script>

<style scoped>
img {
  border-radius: 10px;
}
</style>