<template>
  <div class="container">
    <img class="mx-auto mb-6" src="../assets/img/AxaTr.jpg" alt="AXA" />
    <h3>AXA Türkiye Internship Project</h3>
    <hr />
    <div class="my-2">

      <!-- <button class="add-button" @click="$refs.ModalName.openModal()">
        Add Products
      </button> -->
         <button class="add-button" @click="$refs.ModalName.openModal()"><img src="../assets/img/file-earmark-plus.svg" class= "add-product" alt="Adding Button">Add Product</button>

      <modal ref="ModalName">
        <template v-slot:header>
          <h1>Add product</h1>
        </template>

        <template v-slot:body>
          <div class="group">
            <input v-model="model.name" type="text" required />
            <span class="highlight"></span>
            <span class="bar"></span>
            <label>Name</label>
          </div>
          <div class="group">
            <input v-model="model.stock" type="number" min="1" required />
            <span class="highlight"></span>
            <span class="bar"></span>
            <label>Stock</label>
          </div>
          <div class="group">
            <input v-model="model.price" type="number" min="1" required />
            <span class="highlight"></span>
            <span class="bar"></span>
            <label>Price</label>
          </div>
          <div class="group catagory">
            <label for="products"></label>
            <select v-model="model.categoryId" name="products" id="products">
              <option value="1">Pencil</option>
              <option value="2">Book</option>
            </select>
          </div>
        </template>

        <template v-slot:footer>
          <div class="d-flex align-items-center justify-content-between">
            <button class="btn1" @click="$refs.ModalName.closeModal()">
              Cancel
            </button>
            <button class="btn btn--primary" @click="onSave">Save</button>
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
    <div class="my-4 relative flex items-center">
      <span class="absolute right-8 text-xs">{{ products.length  }} Items Exist</span>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Modal from "./Modal.vue";
export default {
  components: {
    Modal,
  },
  props: {
    item: {
      type: Object,
      default: () => {},
    },
    products: {
      type:Array,
      default: () => []
    }
  },
  data() {
    return {
      model: {
        categoryId:"1", // Default olarak gelir. Çünkü model.
        isDeleted:false
      },
      
    };
  },
  methods: {
   async onSave() {
     this.model.categoryId=parseInt(this.model.categoryId)
      console.log(this.model);
     let result = await axios
        .post("https://localhost:44375/api/products", this.model)
        console.log(result)
        if(result.status==200){
          this.$emit("result", result.data)
          this.$refs.ModalName.closeModal()
        }
    },
  },
  computed: {
    itemCount() {
      return this.products.length || 0;
    },
  },
};
</script>
<style scoped>
.btn1 {
  padding: 8px 16px;
  border-radius: 5px;
  background-color: red;
  color: #fff;
}
.btn {
  padding: 8px 16px;
  border-radius: 5px;
  background-color: green;
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
h1 {
  font-size: 2em;
  font-weight: bold;
}
.group {
  position: relative;
  margin-bottom: 45px;
}
input {
  font-size: 18px;
  padding: 10px 10px 10px 5px;
  display: block;
  width: 300px;
  border: none;
  border-bottom: 1px solid #757575;
}
input:focus {
  outline: none;
}

/* LABEL ======================================= */
label {
  color: #999;
  font-size: 18px;
  font-weight: normal;
  position: absolute;
  pointer-events: none;
  left: 5px;
  top: 10px;
  transition: 0.2s ease all;
  -moz-transition: 0.2s ease all;
  -webkit-transition: 0.2s ease all;
}

/* active state */
input:focus ~ label,
input:valid ~ label {
  top: -10px;
  font-size: 14px;
  color: #5264ae;
}

/* BOTTOM BARS ================================= */
.bar {
  position: relative;
  display: block;
  width: 300px;
}
.bar:before,
.bar:after {
  content: "";
  height: 2px;
  width: 0;
  bottom: 1px;
  position: absolute;
  background: #5264ae;
  transition: 0.2s ease all;
  -moz-transition: 0.2s ease all;
  -webkit-transition: 0.2s ease all;
}
.bar:before {
  left: 50%;
}
.bar:after {
  right: 50%;
}

/* active state */
input:focus ~ .bar:before,
input:focus ~ .bar:after {
  width: 50%;
}

/* HIGHLIGHTER ================================== */
.highlight {
  position: absolute;
  height: 60%;
  width: 100px;
  top: 25%;
  left: 0;
  pointer-events: none;
  opacity: 0.5;
}

/* active state */
input:focus ~ .highlight {
  -webkit-animation: inputHighlighter 0.3s ease;
  -moz-animation: inputHighlighter 0.3s ease;
  animation: inputHighlighter 0.3s ease;
}

/* ANIMATIONS ================ */
@-webkit-keyframes inputHighlighter {
  from {
    background: #5264ae;
  }
  to {
    width: 0;
    background: transparent;
  }
}
@-moz-keyframes inputHighlighter {
  from {
    background: #5264ae;
  }
  to {
    width: 0;
    background: transparent;
  }
}
@keyframes inputHighlighter {
  from {
    background: #5264ae;
  }
  to {
    width: 0;
    background: transparent;
  }
}
#products {
  width: 320px;
  padding: 5px;
  border-radius: 5px;
  border: 1px solid;
}
#products option {
  font-size: 16px;
}
.catagory {
  display: flex;
  justify-content: left;
}
.add-product{
  display: inherit;
  margin-right: 7px;
  margin-top: -3px;
}
</style>