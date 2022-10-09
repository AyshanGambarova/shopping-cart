<template>
  <div id="app">
    <p v-if="isLoading">Loading</p>
    <h2>Shopping Cart</h2>
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
          <a href="#" class="secondary" @click="addToSaved(index)"
            >Save for later</a
          >
        </div>
        <p class="price">${{ item.price }}</p>
      </div>
      <div class="subtotal">
        Subtotal ({{ cart.length }} items):
        <span class="price">${{ cartTotal }}</span>
      </div>
    </div>

    <h2 class="saved-header">Saved for later ({{ saved.length }} item)</h2>
    <div class="cart">
      <div class="item" v-for="(item, index) in saved" :key="index">
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
          <a href="#">Delete</a>
          <a href="#" class="secondary" @click="removeFromSaved(index)"
            >Move to cart</a
          >
        </div>
        <p class="price">${{ item.price }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      isLoading: true,
      cart: [],
      saved: []
    };
  },
  methods: {
    removeFromCart(index) {
      this.cart.splice(index, 1);
    },
    addToSaved(index) {
      const item = this.cart.splice(index, 1);
      this.saved.push(item[0]);
    },
    removeFromSaved(index) {
      const item = this.saved.splice(index, 1);
      this.cart.push(item[0]);
    }
  },
  computed: {
    cartTotal() {
      let total = 0;
      this.cart.forEach(item => {
        total += parseFloat(item.price,10);
      });
      return total.toFixed(2);
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
