<template>
  <!-- Getting the My Data  -->
  <div id="Products">
    <div class="container">
      <table class="table table-striped table-hover" id="my-table">
        <thead>
          <tr>
            <th scope="col">ID</th>
            <th scope="col">Name</th>
            <th scope="col">Stock</th>
            <th scope="col">Price</th>
            <th scope="col">Category</th>
            <th scope="col">Edit</th>
            <th scope="col">Delete</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(product, index) in products" :key="index">
            <th scope="row">{{ product.Id }}</th>
            <td>{{ product.Name }}</td>
            <td>{{ product.Stock }}</td>
            <td>{{ product.Price }}</td>
            <td>{{ product.Category.Name }}</td>
            <td>
              <button class="button" @click="Edit(product)">
                <img src="../assets/img/pencil-square.svg" alt="Edit Button" />
              </button>
            </td>
            <td>
              <button class="button" @click="askForRemove(product.Id, product.Name, index)">
                <img src="../assets/img/trash.svg" alt="Delete Button" />
              </button>
            </td>
          </tr>
        </tbody>
      </table>
      <div class="my-2">
        <modal ref="EditModal">
          <template v-slot:header>
            <h1>Update product</h1>
          </template>

          <template v-slot:body>
            <div class="group">
              <input v-model="model.Name" type="text" required />
              <span class="highlight"></span>
              <span class="bar"></span>
              <label>Name</label>
            </div>
            <div class="group">
              <input v-model="model.Stock" type="number" min="1" required />
              <span class="highlight"></span>
              <span class="bar"></span>
              <label>Stock</label>
            </div>
            <div class="group">
              <input
                v-model="model.Price"
                type="number"
                step="0.01"
                min="1"
                required
              />
              <span class="highlight"></span>
              <span class="bar"></span>
              <label>Price</label>
            </div>
            <div class="group catagory">
              <label for="products"></label>
              <select v-model="model.CategoryId" name="products" id="products">
                <option value="1">Pencil</option>
                <option value="2">Book</option>
              </select>
            </div>
          </template>

          <template v-slot:footer>
            <div class="d-flex align-items-center justify-content-between">
              <button class="btn1" @click="$refs.EditModal.closeModal()">
                Cancel
              </button>
              <button class="btn btn--primary" @click="onSave">Edit</button>
            </div>
          </template>
        </modal>
      </div>
    </div>
    <!-- <p>{{ uyari }}</p> -->
     <div class="overflow-auto">
    <!-- <b-pagination
      v-model="currentPage"
      :total-rows="rows"
      :per-page="perPage"
      aria-controls="my-table"
    ></b-pagination>

    <p class="mt-3">Current Page: {{ currentPage }}</p>

    <b-table
      id="my-table"
      :items="items"
      :per-page="perPage"
      :current-page="currentPage"
      small
    ></b-table> -->
  </div>
  <pagination v-model="page" :records="products.length" :per-page="2" @paginate="Products"/>
  </div>
</template>

<script>
import Modal from "./Modal.vue";
import axios from "axios";
import Pagination from 'v-pagination-3';

export default {
  components: {
    Modal, Pagination
  },
  props: {
    products: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      model: {},
      // uyari: "",
      categories:[],
      page: 1,
    };
  },
  created(){
    //this.getCategories();
  },
  methods: {
     Products: function() {
    console.log("..")
    },
    getCategoriesName(id){
     var res =  this.categories.filter(categorie => categorie.Id == id)
     return res[0].Name;
    },
    getCategories(){
      axios.get(`https://localhost:44375/api/categories`).then((r)=>{
        console.log(r.data)
        this.categories = r.data;
      })
    },
    async Edit(item) {
      Object.assign(this.model, item);
      console.log(this.model);
      this.$refs.EditModal.openModal();
    },

    async onSave() {
      // For editing.
      console.log(this.model);
      let data = {
        id: this.model.Id,
        name: this.model.Name,
        stock: this.model.Stock,
        price: this.model.Price,
        categoryId: this.model.CategoryId,
      };
      console.log(data);
      await axios
        .put(`https://localhost:44375/api/products`, this.model)
        .then((response) => {
          this.$emit('editedProduct');
           this.$refs.EditModal.closeModal()
          console.log(response);
        });
    },
    askForRemove(productId, productName, index) {
      this.$swal
        .fire({
          title:
            productId +
            " id numaralı " + productName + " kaydı sİlmek istediğinizden emin misiniz ?",
            
          showCancelButton: true,

          confirmButtonText: "Sil",
          cancelButtonText: "İptal",
        })
        .then((result) => {
          if (result.isConfirmed) {
            this.removeItem(productId, index);
          }
        });
    },
    async removeItem(productId, index) {
      await axios
        .delete(`https://localhost:44375/api/products/${productId}`)
        .then(() => {
          this.$swal.fire(`${productId} no'lu item silindi`, "", "success");
          // this.uyari = `${productId} no'lu item silindi`;
          let removeItems = this.products;
          removeItems.splice(index, 1);
        })
        .catch(function (error) {
          this.$swal.fire('Silme işlemi başarısız.','','error')
          console.log(error);
        });
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
</style>

