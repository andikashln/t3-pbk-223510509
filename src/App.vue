<template>
  <div class="app">
    <h1>To Do List Saya</h1>
    <input type="text" v-model="newItem" placeholder="Telusuri To do List Saya" @keyup.enter="addItem">
    <table>
      <tr>
      </tr>
      <tr v-for="(item, index) in items" :key="index">
        <td>
          <!-- Tambahkan kotak centang untuk setiap item -->
          <input type="checkbox" v-model="item.completed" class="checkbox">
          <span :class="{ 'completed': item.completed }">{{ item.text }}</span>
          <input v-if="item.editing" type="text" v-model="item.updatedText" @keyup.enter="saveEdit(index)" @blur="cancelEdit(index)">
        </td>
        <td>
          <button @click="editItem(index)">Edit</button>
          <button @click="deleteItem(index)">Delete</button>
        </td>
      </tr>
    </table>
    <!-- Tambahkan input gambar untuk menampilkan kotak -->
    <input type="file" accept="image/*" @change="handleImageUpload">
    <div v-if="imageUrl" class="image-preview">
      <img :src="imageUrl" alt="Image Preview">
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newItem: '',
      items: [],
      imageUrl: ''
    };
  },
  methods: {
    addItem() {
      if (this.newItem.trim() !== '') {
        this.items.push({ text: this.newItem, completed: false });
        this.newItem = '';
      }
    },
    editItem(index) {
      // Tandai item yang diedit
      this.items[index].editing = true;
      // Simpan teks asli untuk pengeditan
      this.items[index].updatedText = this.items[index].text;
    },
    saveEdit(index) {
      // Perbarui teks item dan nonaktifkan mode pengeditan
      if (this.items[index].updatedText.trim() !== '') {
        this.items[index].text = this.items[index].updatedText;
        this.items[index].editing = false;
      }
    },
    cancelEdit(index) {
      // Batal mode pengeditan dan kembalikan teks ke teks asli
      this.items[index].editing = false;
      this.items[index].updatedText = this.items[index].text;
    },
    deleteItem(index) {
      this.items.splice(index, 1);
    },
    handleImageUpload(event) {
      const file = event.target.files[0];
      this.imageUrl = URL.createObjectURL(file);
    }
  }
};
</script>

<style scoped>
.app {
  font-family: Arial, sans-serif;
  max-width: 400px;
  margin: 0 auto;
  background: linear-gradient(135deg, #000000 0%, #ffffff 100%);
  padding: 50px;
  border-radius: 10px;
  box-shadow: 0px 0px 10px 0px rgba(0,0,0,0.2);
}
input[type="text"] {
  width: 100%;
  padding: 8px;
  margin-bottom: 10px;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  margin-bottom: 5px;
}
button {
  margin-left: 5px;
  background-color: #f44336;
  border: none;
  color: white;
  padding: 5px 10px;
  border-radius: 5px;
  cursor: pointer;
}
button:hover {
  background-color: #ca0e0e;
}
.completed {
  text-decoration: line-through;
}
.checkbox {
  margin-right: 5px;
}
.image-preview img {
  max-width: 100%;
  max-height: 200px;
  margin-top: 10px;
}
</style>
