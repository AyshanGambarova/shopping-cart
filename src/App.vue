<template>
  <div id="app">
    <p v-if="isLoading">Loading</p>
    <Cart :cart='cart' title='Shopping Cart' type='shoppingCart' @handleItemChange='handleItemChange'/>
  <Cart :cart='saved' title='Saved for later' type='savedCart' @handleItemChange='handleItemChange'/>
  </div>
</template>

<script>
import Cart from './components/Cart.vue';
export default {
  components: { Cart },
  name: "app",
  data() {
    return {
      isLoading: true,
      cart: [],
      saved: []
    };
  },
  methods: {
   handleItemChange(item,cartType){
     if (cartType==='shoppingCart') {
        this.saved.push(item);
       }
       else{
        this.cart.push(item);
       }
   }
  },
  
  created() {
    fetch(`./data.json`)
      .then(res => {
        return res.json();
      })
      .then(res => {
        this.isLoading = false;
        this.cart = res.cart;
        this.saved = res.saved;
      });
  }
};
</script>

<style>
body {
  font-size: 13px;
  line-height: 19px;
  color: #111;
  font-family: Arial, sans-serif;
  background: #fff;
}

h4,
p {
  padding: 0;
  margin: 0;
}

.item {
  border-bottom: 1px solid #ddd;
  padding: 15px 0;
  overflow: hidden;
}

.item:first-child {
  border-top: 1px solid #ddd;
}

.item img {
  width: 100px;
  height: 100px;
}

h4 {
  color: #0066c0;
  font-size: 16px;
  line-height: 1.255;
}

.image,
.info {
  float: left;
}

.image {
  margin-right: 20px;
}

.seller {
  font-size: 13px;
  line-height: 19px;
}

.status.available {
  color: #008a00;
  font-size: 12px;
  line-height: 1.5;
}

.shipping {
  color: #555;
  font-size: 12px;
  line-height: 1.5;
}

.info a {
  color: #0066c0;
  font-size: 12px;
  text-decoration: none;
  line-height: 24px;
}

.saved-header {
  margin-top: 50px;
}

a.secondary {
  padding-left: 5px;
  margin-left: 3px;
  border-left: 1px solid #ccc;
}

.price {
  color: #b12704;
  font-size: 17px;
}

p.price {
  float: left;
  margin-left: 20px;
}

.subtotal {
  font-size: 17px;
  font-weight: bold;
  line-height: 60px;
  text-align: right;
  margin-right: 40px;
}
</style>
