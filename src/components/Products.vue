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
      <td><button class="button" @click="$refs.EditModal.openEditModal()"><img src="../assets/img/pencil-square.svg" alt="Edit Button"></button></td>
           <EditModal ref="EditModal">
      <template v-slot:header>
        <h1>Add product</h1>
      </template>

      <template v-slot:body>
        <div>
          <label>Name:</label>
          <input class="border-2 border-black" type="text">
        </div>
        <div>
          <label>Stock:</label>
          <input id="stock" type="text">
        </div>
        <div>
          <label>Price:</label>
          <input id="price" type="number">
        </div>
        <div>
          <label for="products">Catagory:</label>
          <input id="products" class="border-2 border-black" type="text">
        </div>
        
          

    <select name="products" id="products">
      <option value="volvo">Pencil</option>
      <option value="saab">Book</option>
    </select>
      </template>
           </EditModal>
      <td><button class="button"><img src="../assets/img/trash.svg" alt="Delete Button"></button></td>
    </tr>
  </tbody>
</table>
    </div>
  </div>
</template>

<script>
import EditModal from "./EditModal.vue"
export default {
  components: {
    EditModal
  },
  props:{
    products:{
      type : Array,
      default : () => []
    }
  },
  data () {
    return  {
    };
  },
 
};
</script>


