<template>
  <section>
    <div class="form-container">
      <h1>Add Product</h1>
      <form @submit.prevent="addProduct">
        <div>
          <label>Name</label>
          <br />
          <input type="text" v-model="state.input" />
        </div>
        <div>
          <button type="submit" class="submit">Add Product</button>
        </div>
      </form>
    </div>
    <div class="list-container">
      <ul>
        <li v-for="(Product, index) in state.Products" :key="index">
          <template v-if="index !== state.editingIndex">
            <div>
              {{ Product }}
              <span style="float:right;padding-right:10px;">
                <button @click="removeProduct(index)">X</button>
                <button @click="startEditing(index)">Edit</button>
              </span>
            </div>
          </template>
          <template v-else>
            <div>
              <input type="text" :value="state.editingProduct" @input="updateEditingProduct($event.target.value)" />
              <button @click="saveEditingProduct(index)">Save</button>
              <button @click="cancelEditing()">Cancel</button>
            </div>
          </template>
        </li>
      </ul>
    </div>
  </section>
</template>

<script>
import { reactive } from "vue";

export default {
  setup() {
    const { state, addProduct, removeProduct, startEditing, updateEditingProduct, saveEditingProduct, cancelEditing } = ProductList();
    return { state, addProduct, removeProduct, startEditing, updateEditingProduct, saveEditingProduct, cancelEditing };
  }
};

function ProductList() {
  const state = reactive({
    input: "",
    Products: ["Product 1"],
    editingIndex: -1,
    editingProduct: "",
  });

  const addProduct = () => {
    state.Products.push(state.input);
    state.input = "";
  };

  const removeProduct = (i) => {
    state.Products.splice(i, 1);
  };

  const startEditing = (i) => {
    state.editingIndex = i;
    state.editingProduct = state.Products[i];
  };

  const updateEditingProduct = (value) => {
    state.editingProduct = value;
  };

  const saveEditingProduct = (i) => {
    state.Products[i] = state.editingProduct;
    state.editingIndex = -1;
  };

  const cancelEditing = () => {
    state.editingIndex = -1;
    state.editingProduct = "";
  };

  return { state, addProduct, removeProduct, startEditing, updateEditingProduct, saveEditingProduct, cancelEditing };
}
</script>
<style scoped>
input {
  width: 20%;
  height: 30px;
  border: 2px solid green;
}
.submit {
  margin: 10px;
  padding: 10px;
  border-radius: 0px;
  border: 0px;
  background: green;
  color: white;
}
ul li {
  list-style: none;
  border: 2px solid green;
  width: 30%;
  margin-top: 10px;
}
</style>
