<template>
  <div>
      <div>
        <div>
          <div class="title__container">
            <span>Productos</span>
          </div>
          <button @click="addProduct"><i class="fa-solid fa-plus"></i> Agregar Producto</button>
        </div>
        <table>
          <thead>
            <tr>
              <th>Name</th>
              <th>Description</th>
              <th>Price</th>
              <th>Acciones</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="product in products" :key="product.id">
              <td>{{ product.name }}</td>
              <td>{{ product.description }}</td>
              <td>{{ product.price }}</td>
              <td>
                <button @click="editProduct(product.id)">Editar</button>
                <button @click="deleteProduct(product.id)">Eliminar</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
  </div>
</template>
<script>
export default {
  name: 'App',
  data() {
    return {
      apiUrl: useRuntimeConfig().public.API_URL,
      products: [],
    }
  },
  mounted() {
    this.getProducts();
  },
  methods: {
    async getProducts() {
      try {
        const data  = await $fetch(this.apiUrl + '/Products');
        this.products = data;
      } catch (error) {
        console.error(error);
      }
    }
  }
}
</script>
<style>
body {
  font-family: 'Roboto', sans-serif !important;
}
.title__container {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  background-color: #f0f0f0;
  border-radius: 5px;
  margin-bottom: 10px;
  font-size: 24px;
  font-family: 'Roboto', sans-serif;
  color: #8a8a8a;
}
table {
  width: 100%;
  border-collapse: collapse;
}
</style>