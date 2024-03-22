<template>
  <div class="item-list">
    <div v-for="(item, index) in items" :key="index" class="item">
      <div class="item-details">
        <p class="item-name">{{ item.name }}</p>
        <p class="item-price">$ {{ item.price }}</p>
      </div>
      <div class="button-group">
        <button @click="addToCart(item)" class="add-to-cart-btn">Add to Cart</button>
        <button @click="updateItem(index)" class="update-btn">Update</button>
        <button @click="removeItem(index)" class="remove-btn">Remove</button>
      </div>
    </div>
    <br>
    <button @click="addItem" class="add-item-btn">Add New Item</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [
        { name: 'Adidas Samba OG Cloud White Core Black', price: 153 },
        { name: 'Adidas Samba OG Black White Gum', price: 79 },
        { name: 'Jordan 1 High Chicago', price: 36000 },
        { name: 'Jordan 1 Retro High OG x Travis Scott Mocha', price: 1620 },
        { name: 'Jordan 1 Retro High OG x Dior', price: 7026 },
        { name: 'Jordan 3 Retro White Cement Reimagined', price: 318 },
        { name: 'Jordan 4 Retro x Manila', price: 14500 },
        { name: 'New Balance 530 Grey Matter Silver Metallic', price: 82 },
        { name: 'New Balance 550 White Green', price: 100 },
        { name: 'New Balance 550 White Grey', price: 114 },
        { name: 'Nike Air Force 1 Low 07 White', price: 91 },
        { name: 'Nike Dunk Low Retro White Black Panda', price: 96 },
        { name: 'Nike Dunk Low SP St. Johns', price: 162 },
        { name: 'Nike Dunk Low UNC', price: 101 },
        { name: 'Nike SB Dunk Low x The Powerpuff Girls Blossom', price: 290 }
      ]
    };
  },
  methods: {
    addToCart(item) {
      this.$emit('add-to-cart', item);
    },
    updateItem(index) {
      const item = this.items[index];
      const newName = prompt('Enter the new name:', item.name);
      const newPrice = parseFloat(prompt('Enter the new price:', item.price));
      
      if (newName && !isNaN(newPrice)) {
        item.name = newName;
        item.price = newPrice;
      }
    },
    removeItem(index) {
      this.items.splice(index, 1);
    },
    addItem() {
      const name = prompt('Please enter the name of the new item:');
      const price = parseFloat(prompt('Please enter the price of the new item:'));
      
      if (name && !isNaN(price)) {
        this.items.push({ name, price });
      }
    }
  }
};
</script>

<style scoped>
.item-list {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 20px;
}

.item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #ccc;
  width: 80%;
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

.item-price {
  margin: 0;
  font-family: Tahoma;
}

.button-group {
  display: flex;
  gap: 10px;
}

.add-to-cart-btn, .add-item-btn {
  background-color: #10a200;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.update-btn {
  background-color: blue;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}
.remove-btn {
  background-color: red;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}
.add-to-cart-btn:hover, .update-btn:hover, .remove-btn:hover, .add-item-btn:hover {
  background-color: #3a403b;
}
</style>
