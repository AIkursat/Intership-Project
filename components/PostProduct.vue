<template>
  <div class="container">
    <h3>AXA Türkiye Internship Project</h3>
    <hr />
    <div class="my-2">
      <!-- <input type="text" placeholder="What will you buy?" @keydown.enter="onSave" /> -->
    
   <button class="add-button" @click="$refs.modalName.openModal()">Add Products</button>
      <modal ref="modalName">
      <template v-slot:header>
        <h1>Add product</h1>
      </template>

      <template v-slot:body>
        <div class="mb-2">
          <label for= "name">Name:</label>
          <input id ="name" class="border-2 border-black rounded" type="text">
        </div>

        <div class="mb-2">
          <label for ="stock">Stock:</label>
          <input id = "stock" class="border-2 border-black rounded" type="number" min="1">
        </div>

        <div class="mb-2">
          <label for="price">Price:</label>
          <input id = "price" class="border-2 border-black rounded" type="number" min="1">
        </div>

        <div class="mb-2">
          <label for="products">Catagory:</label>
          <select class="border-2 border-black rounded" name="products" id="products">
            <option value="volvo">Pencil</option>
            <option value="saab">Book</option>
          </select>
        </div>
        
        
        

        
      </template>

      <template v-slot:footer>
        <div class="d-flex align-items-center justify-content-between">
          <button type="button" class="btn1 btn-outline-warning" @click="$refs.modalName.closeModal()">Cancel</button>
          <button type="button" class="btn btn--primary" @click="$refs.modalName.closeModal()">Save</button>
        </div>
      </template>
    </modal>
    </div>
    <!-- <ul v-if="products.length > 0">
      <li v-for="product in products" :key="product.Id" class="d-flex justify-content-between align-items-center">      
      </li>
    </ul> 
    <div v-else class="bg-blue text-white">
      No Items
    </div> -->
    <div class="my-4 relative flex items-center"><span class="absolute right-8 text-xs">{{ itemCount }} Items Exist</span></div>
  </div>
</template>

<script>
import axios from "axios";
import Modal from "./Modal.vue"
export default {
  components: {
    Modal
  },
  data() {
    return {
      products: []
    };
  },
  mounted() {
    axios.get("https://localhost:44375/api/products").then(items_response => {
      console.log("items_response :>> ", items_response);
      this.products = items_response.data || [];
      console.log("this.itemsList :>> ", this.products);
    });
  },
  methods: {
    onSave(e) {
      const saveObject = {
        Name: e.target.value,
        Stock : e.target.value,
        price : e.target.value,
        CategoryId : e.target.value
      };
      axios.post("https://localhost:44375/api/products", saveObject).then(save_response => {
        console.log("Save ",save_response);
        this.$emit("init")
        e.target.value = "";
        e.target.focus();
      }).catch(err => {
          console.log(err);
      });
    },
    onDelete(product) {
      axios.delete(`https://localhost:44375/api/products/${product.Id}`).then(delete_response => {
        console.log(delete_response);
        this.products = this.products.filter(i => i.Id !== product.Id);
      });
    }
  },
  computed: {
    itemCount() {
      return this.products.length || 0;
    }
  }
};
</script>
<style>
.btn {
  padding: 8px 16px;
  border-radius: 5px;
    background-color: green;
    color: #fff;
}
.btn1{
  padding: 8px 16px;
  border-radius: 5px;
  background-color: red;
  color: #fff;
}
.overflow-hidden {
  overflow: hidden;
}
.mx-auto {
  margin-left: auto;
  margin-right: auto;
}
.d-flex {
  display: flex;
}
.align-items-center {
  align-items: center;
}
.justify-content-between {
  justify-content: space-between;
}
button {
  background: none;
  border: none;
  outline: inherit;
  cursor: pointer;
}

h1,
h2,
h3,
h4,
h5,
h6 {
  margin: 0;
}
</style>