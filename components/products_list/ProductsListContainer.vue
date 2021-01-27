<template>
  <div class="columns is-centered is-multiline">
    <div class="card column is-one-quarter" v-for="product in products" :key="product.id">
      <VmProducts :product="product"></VmProducts>
    </div>
    <div class="section" v-if="products.length === 0">
      <p>{{ noProductLabel }}</p>
    </div>
  </div>
</template>

<script>
import VmProducts from '../Products';
import { getByTitle } from '@/assets/filters';
import axios from 'axios'

export default {
  name: 'productsList',

  components: { VmProducts },

  data () {
    return {
      id: '',
      noProductLabel: 'No product found',
      productsFiltered: [],
      productsJSON: []
    };
  },

  mounted () {
    axios
      .get(process.env.GOVUE_API_BASE + "/product/")
      .then(response => {
        console.log(response)
        console.log(response.data.products)
        this.productsJSON =  response.data.products
      })
  },

  computed: {
    products () {
      if (this.$store.state.userInfo.hasSearched) {
        return this.getProductByTitle();
      } else {
          return this.productsJSON;
      }
    }
  },

  methods: {
    getProductByTitle () {
      let listOfProducts = this.productsJSON,
          titleSearched = this.$store.state.userInfo.productTitleSearched;
          return this.productsFiltered = getByTitle(listOfProducts, titleSearched);
    }
  }

};
</script>

<style lang="scss" scoped>
  .card {
    margin: 10px;
  }
</style>
