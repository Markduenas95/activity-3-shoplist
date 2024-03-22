<template>
  
  <div id="app">
    <center><h2 style="font-family: Tahoma">Item List</h2></center>

    <item-list @add-to-cart="addToCart"></item-list>

    <h2 style="font-family: Tahoma">My Shopping Cart</h2>
    <shopping-cart :cart="cart" @update-quantity="updateQuantity" @remove-from-cart="removeFromCart"></shopping-cart>
  </div>
</template>

<script>
import ItemList from './components/itemList.vue';
import ShoppingCart from './components/shoppingCart.vue';

export default {
  components: {
    ItemList,
    ShoppingCart
  },
  data() {
    return {
      cart: []
    };
  },
  methods: {
    addToCart(item) {
      const existingItemIndex = this.cart.findIndex(i => i.name === item.name);
      if (existingItemIndex !== -1) {
        this.cart[existingItemIndex].quantity++;
      } else {
        this.cart.push({ ...item, quantity: 1 });
      }
    },
    updateQuantity({ index, quantity }) {
      this.cart[index].quantity = quantity;
      if (this.cart[index].quantity <= 0) {
        this.cart.splice(index, 1);
      }
    },
    removeFromCart(index) {
      this.cart.splice(index, 1);
    }
  }
};
</script>
