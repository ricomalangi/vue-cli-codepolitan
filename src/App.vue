<template>
  <div id="app" class="container mt-4">
    <h1>IDShop</h1>
    <navbar :cart="cart" :cartQty="cartQty" :cartTotal="cartTotal" @toggle="toggleSliderStatus"></navbar>
    <price-slider :sliderStatus="sliderStatus" :maximum.sync="maximum"></price-slider>
    <product-list :products="products" :maximum="maximum" @add="addItem"></product-list>
  </div>
</template>

<script>
import Navbar from './components/Navbar.vue'
import PriceSlider from './components/PriceSlider.vue'
import ProductList from './components/ProductList.vue'
export default {
  name: 'App',
  data: function(){
    return {
      maximum: 50,
      products: [],
      cart: [],
      sliderStatus: false
    }
  },
  components: {
    ProductList,
    Navbar,
    PriceSlider,
  },
  computed: {
    toggleSliderStatus: function(){
      this.sliderStatus = !this.sliderStatus
    },
    cartTotal: function () {
      let sum = 0;
      for (let key in this.cart) {
        sum += this.cart[key].product.price * this.cart[key].qty;
        console.log(key);
      }
      return sum;
    },
    cartQty: function () {
      let qty = 0;
      for (let key in this.cart) {
        qty += this.cart[key].qty;
      }
      return qty;
    },
  },
  mounted: function () {
    fetch("https://hplussport.com/api/products/order/price")
      .then((response) => response.json())
      .then((data) => {
        this.products = data;
      });
  },
  methods: {
    addItem: function (product) {
      let productIndex;
      let productExist = this.cart.filter(function (item, index) {
        if (item.product.id == Number(product.id)) {
          productIndex = index;
          return true;
        } else {
          return false;
        }
      });
      console.log(productExist);
      if (productExist.length) {
        console.log(productIndex);
        this.cart[productIndex].qty++;
      } else {
        console.log("else steatment");
        this.cart.push({ product: product, qty: 1 });
      }
    },
  }
}
</script>
