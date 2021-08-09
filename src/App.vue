<template>
  <svg v-if="loader" class="loader" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" style="margin: auto; background: rgb(255, 255, 255); display: block; shape-rendering: auto;" width="200px" height="200px" viewBox="0 0 100 100" preserveAspectRatio="xMidYMid">
    <circle cx="50" cy="50" fill="none" stroke="#e15b64" stroke-width="1" r="45" stroke-dasharray="212.05750411731105 72.68583470577035">
      <animateTransform attributeName="transform" type="rotate" repeatCount="indefinite" dur="1s" values="0 50 50;360 50 50" keyTimes="0;1"></animateTransform>
    </circle>
    </svg>

  <div class="container col-12">
      <Filters :filters="this.filters"></Filters>
      <Products :products="this.products" :pageNumber="this.pageNumber" :totalCount="this.totalCount" @getMoreData="getMoreData"></Products>
  </div>
</template>

<script>

import Products from './components/Products';
import Filters from './components/Filters';
import axios from 'axios';

export default {
  name: 'App',
  data() {
    return{
      pageNumber: 1,
      response : [],
      products: [],
      filters: [],
      loader: true,
      totalCount:0
    }
  },
  components: {
    Filters,
    Products
  },

  async mounted(){
    await this.getData();
  },

  methods:{
    async getData(){
      await axios.get(`https://pim.wforwomanonline.com/pim/pimresponse.php/?service=category&store=1&url_key=top-wear-kurtas&sort_dir=desc&page=${this.pageNumber}&count=20`)
          .then(response => {
            this.filters = response.data.result.filters;
            this.totalCount = response.data.result.count;
            if(this.pageNumber === 1)
              this.products = response.data.result.products;
            else if(this.pageNumber > 1)
              this.products = this.products.concat(response.data.result.products);

            this.loader = false;
          }).catch((e) => {
            console.log(e);
          });
    },

    getMoreData(){
      this.pageNumber = this.pageNumber + 1;
      this.getData();
    }
  }
}
</script>

<style>

.container.col-12{
  margin-top: 50px;
  display: flex;
}

svg.loader {
  margin-top: 240px !important;
}
</style>
