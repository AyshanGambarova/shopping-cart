<template>
  <div>
    <h2>{{ title }}</h2>
    <p v-if="!cart.length">No item in cart.</p>
    <div class="cart">
      <div class="item" v-for="(item, index) in cart" :key="index">
        <div class="image">
          <a :href="item.url">
            <img :src="item.image" />
          </a>
        </div>
        <div class="info">
          <h4>{{ item.name }}</h4>
          <p class="seller">by {{ item.seller }}</p>
          <p class="status available" v-if="item.isAvailable">In Stock</p>
          <p class="shipping" v-if="item.isEligible">
            Eligible for FREE Shipping & FREE Returns
          </p>
          <a href="#" @click="removeFromCart(index)">Delete</a>
          <a href="#" class="secondary" @click="changeCart(index)" v-if="type=='shoppingCart'"
            >Save for later</a
          >
          <a href="#" class="secondary" @click="changeCart(index)" v-if="type=='savedCart'"
            >Move to cart</a
          >
        </div>
        <p class="price">${{ item.price }}</p>
      </div>
    <div class="subtotal" v-if="cart.length">
        Subtotal ({{ cart.length }} items):
        <span class="price">${{ cartTotal }}</span>
      </div> 
    </div>
  </div>
</template>
<script>
export default {
  props: ["cart", "title",'type'],
    methods: {
    removeFromCart(index) {
     return this.cart.splice(index, 1);
    },
    changeCart(index){
       let item= this.removeFromCart(index);
       this.$emit('handleItemChange',item[0],this.type);
    }
  
  },
  computed: {
    cartTotal() {
      let total = 0;
      this.cart.forEach(item => {
        total += parseFloat(item.price, 10);
      });
      return total.toFixed(2);
    }
  }
};
</script>
<style></style>
