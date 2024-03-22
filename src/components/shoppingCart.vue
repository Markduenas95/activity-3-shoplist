<template>
  <div class="cart">
    <!-- Cart items -->
    <div v-for="(item, index) in cart" :key="index" class="cart-item">
      <div class="item-details">
        <p class="item-name">{{ item.name }}</p>
        <p class="item-price">$ {{ item.price }}</p>
        <p class="item-quantity">Quantity: {{ item.quantity }}</p>
      </div>
      <div class="quantity-controls">
        <button @click="openDialog(index)" class="cart-update-btn">Update</button>
        <button @click="removeFromCart(index)" class="cart-remove-btn">Remove</button>
      </div>
    </div>

    <!-- Total -->
    <p class="total">Total Amount: $ {{ total }}</p>

    <!-- Dialog for updating quantity -->
    <div v-if="dialogVisible" class="dialog">
      <div class="dialog-content">
        <p>Enter new quantity for {{ cart[selectedItemIndex].name }}:</p>
        <input type="number" v-model="newQuantity" @keydown.enter="updateQuantity(selectedItemIndex)">
        <button @click="updateQuantity(selectedItemIndex)" class="cart-save-btn">Save</button>
        <button @click="closeDialog" class="cart-cancel-btn">Cancel</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dialogVisible: false,
      selectedItemIndex: null,
      newQuantity: '', // Use newQuantity to bind the input value
    };
  },
  props: {
    cart: {
      type: Array,
      default: () => [],
    },
  },
  computed: {
    total() {
      if (this.cart.length === 0) {
        return 0; // Return 0 if the cart is empty
      }
      return this.cart.reduce((acc, item) => acc + item.price * item.quantity, 0);
    },
  },
  methods: {
    openDialog(index) {
      this.selectedItemIndex = index;
      // Initialize newQuantity to the quantity of the selected item
      this.newQuantity = this.cart[index].quantity.toString(); // Set as string to handle empty input
      this.dialogVisible = true;
    },
    closeDialog() {
      this.dialogVisible = false;
    },
    updateQuantity(index) {
      // Parse the input value as an integer directly within the input handler
      const newQuantity = parseInt(this.newQuantity);

      // Check if the parsed value is a valid integer and non-negative
      if (!isNaN(newQuantity) && newQuantity >= 0) {
        // Emit an event to notify the parent component about the quantity update
        this.$emit('update-quantity', { index, quantity: newQuantity });
        // Close the dialog
        this.closeDialog();
      } else {
        // Show an alert if the input is not valid
        alert('Please enter a valid non-negative integer!');
      }
    },
    removeFromCart(index) {
      this.$emit('remove-from-cart', index);
    },
  },
};
</script>

<style scoped>
.cart {
  margin-top: 20px;
}

.cart-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #ccc;
  font-family: Tahoma;
}

.item-details {
  flex: 1;
  font-family: Tahoma;
}

.item-name {
  margin: 0;
  font-weight: bold;
  font-family: Tahoma;
}

.item-quantity {
  margin: 0;
  font-family: Tahoma;
}

.item-price {
  margin: 0;
  font-family: Tahoma;
}

.quantity-controls {
  display: flex;
  align-items: center;
  font-family: Tahoma;
}

button {
  margin-left: 10px;
  font-family: Tahoma;
}

.cart-update-btn {
  font-family: Tahoma;
  background-color: blue;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.cart-remove-btn {
  font-family: Tahoma;
  background-color: red;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.cart-update-btn:hover, .cart-remove-btn:hover {
  background-color: #3a403b;
}

.cart-save-btn {
  font-family: Tahoma;
  background-color: blue;
  color: white;
  padding: 5px 16px;
  border-radius: 4px;
  border: none;
  cursor: pointer;
  transition: background-color 0.3s;
}

.cart-cancel-btn {
  font-family: Tahoma;
  background-color: red;
  color: white;
  padding: 5px 12px;
  border-radius: 4px;
  border: none;
}

.cart-save-btn:hover, .cart-cancel-btn:hover {
  background-color: #3a403b;
}

.total {
  font-size: 18px;
  font-weight: bold;
  font-family: Tahoma;
  margin-top: 10px;
}

.dialog {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  font-family: Tahoma;
  display: flex;
  justify-content: center;
  align-items: center;
}

.dialog-content {
  background-color: white;
  padding: 20px;
  border-radius: 5px;
  font-family: Tahoma;
}

.dialog-content input {
  width: 100px;
  font-family: Tahoma;
}
</style>
