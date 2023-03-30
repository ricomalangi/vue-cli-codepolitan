<template>
  <transition-group name="fade" tag="div" @beforeEnter="before" @enter="enter" @leave="leave">
    <div class="row d-none" v-for="(item, i) in showItem" :key="i" :data-index="i">
      <div class="col-1 m-auto">
        <button class="btn btn-info" @click="$emit('add', item)">+</button>
      </div>
      <div class="col-sm-4">
        <img :src="item.image" :alt="item.name" class="img-fluid d-block">
      </div>
      <div class="col">
        <h2 class="text-info">{{ item.name }}</h2>
        <p class="mb-0">{{ item.description }}</p>
        <div class="h5 float-right">
          <price :value="Number(item.price)"></price>
        </div>
      </div>
    </div>
  </transition-group>
</template>
<script>
import Price from "./Price.vue"
export default {
  name: "product-list",
  components: {
    Price
  },
  props: ["products", "maximum"],
  computed: {
    showItem: function(){
      let max = this.maximum
      return this.products.filter(function(item){
        return item.price <= max
      })
    }
  },
  methods: {
    before: function (el) {
      el.className = "d-none";
    },
    enter: function (el) {
      let delay = el.dataset.index * 100;
      setTimeout(() => {
        el.className =
          "row d-flex mb-3 align-items-center animate__animated animate__zoomIn";
      }, delay);
    },
    leave: function (el) {
      let delay = el.dataset.index * 100;
      setTimeout(() => {
        el.className =
          "row d-flex mb-3 align-items-center animate__animated animate__zoomOut";
      }, delay);
    },
  },
}
</script>