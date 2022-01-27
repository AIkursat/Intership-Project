<template>
<!-- Getting the My Data  -->
  <div> 
    <div class="container">
      <table class="table mx-auto w-2/3">
  <thead>
    <tr>
      <th scope="col">ID</th>
      <th scope="col">Name</th>
      <th scope="col">Stock</th>
      <th scope="col">Price</th>
      <th scope="col">Edit</th>
      <th scope="col">Delete</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="product in products" :key="product.Id">
      <th scope="row">{{product.Id}}</th>
      <td>{{product.Name}}</td>
      <td>{{product.Stock}}</td>
      <td>{{product.Price}}</td>
      <td><button class="button" @click="Edit(product)"><img src="../assets/img/pencil-square.svg" alt="Edit Button"></button></td> 
      <td><button class="button"><img src="../assets/img/trash.svg" alt="Delete Button"></button></td>
    </tr>
  </tbody>
</table>
 <div class="my-2">
      <!-- <input type="text" placeholder="What will you buy?" @keydown.enter="onSave" /> -->

    
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
            <input v-model="model.Price" type="number" min="1" required />
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
            <button class="btn1" @click="$refs.ModalName.closeModal()">
              Cancel
            </button>
            <button class="btn btn--primary" @click="onSave">Save</button>
          </div>
        </template>
      </modal>
    </div>
    </div>
  </div>
</template>

<script>
import Modal from "./Modal.vue";
export default {
  components:{
     Modal,
  },
  props:{
    products:{
      type : Array,
      default : () => []
    }
  },
  data () {
    return  {
      model : {}
    };
  },
 methods:{
   Edit(item) {
     Object.assign(this.model, item)
     console.log(this.model);
     this.$refs.EditModal.openModal()
   }
 }

};
</script>


