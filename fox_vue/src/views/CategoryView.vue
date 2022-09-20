<template>
  <div class="page-category">
    <div class="columns is-multiline">
      <div class="column is-12">
        <h1 class="title is-1 has-text-centered"> {{ category.name }}</h1>
      </div>

      <ProductBox
          v-for="product in category.products"
          v-bind:key="product.id"
          v-bind:product="product"
      >
      </ProductBox>
    </div>
  </div>
</template>


<script>
import axios from "axios";
import {toast} from "bulma-toast";
import ProductBox from "@/components/ProductBox";

export default {
  name: 'CategoryView',
  components: {
    ProductBox
  },

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

  watch: {
    $route(to, from) {
      if (to.name === 'category') {
        this.getCategory()
      }
    }
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
