<template>
  <div class="page-category">
    <div class="columns is-multiline">
      <div class="column is-12">
        <h1 class="title is-1 has-text-centered"> {{ category.name }} color</h1>
      </div>

      <div class="column is-3" v-for="product in category.products" v-bind:key="product.id">
        <div class="box">
          <figure class="image is-3by2">
            <img v-bind:src="product.get_thumbnail" alt="image thumbnail">
          </figure>
          <h3 class="is-size-4">{{ product.name }}</h3>
          <p class="is-size-6 has-text-grey">${{ product.price }}</p>

          <router-link v-bind:to="product.get_absolute_url" class="button is-info is-light mt-4">View details</router-link>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
import axios from "axios";
import {toast} from "bulma-toast";

export default {
  name: 'Category',
  data() {
    return {
      category: {
        products: []
      }
    }
  },
  mounted() {
    this.getCategory()
  },
  methods: {
    async getCategory() {
      const category_slug = this.$route.params.category_slug

      this.$store.commit('setIsLoading', true)

      await axios
          .get(`/api/v1/products/${category_slug}/`)
          .then(response => {
            this.category = response.data

            document.title = this.category.name + ' | Foxes'
          })
          .catch(error => {
            console.log(error)

            toast({
                message: 'Something went wrong! Please try again',
                type: 'is-danger',
                dismissible: false,
                pauseOnHover: true,
                duration: 2000,
                position: 'bottom-right',
                animate: {
                  in: 'fadeIn',
                  out: 'fadeOut'
                }
            })
          })

      this.$store.commit('setIsLoading', false)

    }
  }
}
</script>

<style scoped>
.image {
  margin-top: -1.25rem;
  margin-left: -1.25rem;
  margin-right: -1.25rem;
}

</style>