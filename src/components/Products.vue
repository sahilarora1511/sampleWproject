<template>
  <div class="col-md-10">

    <div class="col-md-12">
      <div class="row item-count">
        <div class="col-md-6"><p class="toolbar-amount"><span class="toolbar-number">{{totalCount}}</span> Item(s)</p>
        </div>
      </div>
    </div>


    <div class="products">
      <div class="col-md-12">
        <div class="row">
      <div class="col-md-3 st-products" v-for="item in products" :key="item.id_product">
        <div class="product-card" data-toggle="tooltip" :title="item.name">

          <div class="top-card">   <!-- Do this -->
            <div class="image-container">
            <a href="#">
              <img :src="item.image" class="image-here">
            </a>
            </div>
          </div>

          <div class="bottom-card">
            <div class="title">
              {{item.name}}
            </div>

            <div class="prices">
              <span class="discounted_price">Rs. {{item.selling_price}}</span>
              <span class="mrp" v-show="item.discount>0">Rs. {{item.price}}</span>
              <span class="discount" v-show="item.discount>0">{{item.discount}}</span>
            </div>

            <div class="sizes">
              Size: {{getSizes(item.size)}}
            </div>

          </div>

        </div>
      </div>
        </div>
      </div>
    </div>

    <div id="footer" v-show="products.length>0">
      Loading....
    </div>

  </div>
</template>

<script>
export default {
  name: "Products",
  props:{
    products: Array,
    pageNumber: Number,
    totalCount: Number
  },

  mounted(){
    let self = this;
    new IntersectionObserver(function (entries) {
      // isIntersecting is true when element and viewport are overlapping
      // isIntersecting is false when element and viewport don't overlap
      if (entries[0].isIntersecting) {
        self.$emit('getMoreData')
      }
    }, {threshold: [0]}).observe(document.querySelector("#footer"));

  },

  methods:{
    getSizes(size){
      let replacedSize = size.replaceAll('[','');
      replacedSize = replacedSize.replaceAll(']','')
      replacedSize = replacedSize.replaceAll(',',' ')
      return replacedSize;
    },

   getMoreResults(){
      this.$emit('getMoreData')
   }
  }
}
</script>

<style scoped>
.products{
  display: flex;
  flex-wrap: wrap;
}

.col-md-3.st-products {
  padding: 8px;
  margin-bottom: 30px;
}

.product-card {
  border: 1px solid #fff;
}

.product-card:hover {
  border: 1px solid #ededed;
}

span.discounted_price {
  margin-right: 5px;
}

span.mrp {
  margin-right: 10px;
  text-decoration: line-through;
}

span.discount {
  color: #bf1616;
}

.sizes{
  opacity: 0;
}

.product-card:hover > .bottom-card .sizes{
  opacity: 1
}

#footer{
  text-align: center;
}

.top-card {
  position: relative;
  overflow: hidden;
  padding-bottom: 150%;
}

.image-here{
  position: absolute;
  width: 100%;
  top: 0;
  left: 0;
  height: 100%;
}

p.toolbar-amount {
  float: left;
  padding: 6px 0 0;
  margin-left: 0;
  margin-bottom: 0;
}

.row.item-count {
  border: 1px solid #ededed;
  padding: 6px 0;
  margin: 10px -15px;
}

</style>
