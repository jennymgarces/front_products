<template>
  <div>
      <div>
        <div>
          <div class="title__container">
            <span>Productos</span>
          </div>
          <button @click="showAddProduct = !showAddProduct"><i class="fa-solid fa-plus"></i> Agregar Producto</button>
        </div>
        <div v-if="showAddProduct" class="add__product__container">
          <div>
            <input class="input__product" type="text" v-model="newProduct.name" placeholder="Nombre">
          </div>
          <div>
            <input class="input__product" type="text" v-model="newProduct.description" placeholder="Descripcion">
          </div>
          <div>
            <input class="input__product" type="number" v-model="newProduct.price" placeholder="Precio">
          </div>
          <div>
            <button v-if="!newProduct.id" class="save__button" @click="saveProduct">Guardar</button>
            <button v-if="newProduct.id" class="save__button" @click="editProducFetch(newProduct.id)">Editar</button>
            <button class="cancel__button" @click="cancelActions">Cancelar</button>
          </div>
        </div>
        
        <table>
          <thead>
            <tr>
              <th>Nombre</th>
              <th>Descripcion</th>
              <th>Precio</th>
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
                <button class="delete__button" @click="deleteProduct(product.id)">Eliminar</button>
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
      showAddProduct: false,
      newProduct: {
        name: '',
        description: '',
        price: 0,
      },
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
    },
    async addProduct() {
      this.products.push({
        name: '',
        description: '',
        price: 0,
      });
    },
    async saveProduct() {
      try {
        const data = {
          name: this.newProduct.name,
          description: this.newProduct.description,
          price: this.newProduct.price,
          isActive: true,
        };
        await $fetch(this.apiUrl + '/Products', {
          method: 'POST',
          body: data,
        });
        this.getProducts();
        this.showAddProduct = false;
      } catch (error) {
        console.error(error);
      }
    },
    async editProduct(id) {
      this.showAddProduct = true;
      this.newProduct = this.products.find(product => product.id === id);
    },
    async editProducFetch(id) {
      const data = {
        name: this.newProduct.name,
        description: this.newProduct.description,
        price: this.newProduct.price,
        isActive: true,
      };
      try {
        await $fetch(this.apiUrl + '/Products/' + id, {
          method: 'PUT',
          body: data,
        });
        this.getProducts();
        this.showAddProduct = false;
        this.newProduct = {
          name: '',
          description: '',
          price: 0,
        };
      } catch (error) {
        console.error(error);
      }
    },
    async deleteProduct(id) {
      try {
        await $fetch(this.apiUrl + '/Products/' + id, {
          method: 'DELETE',
        });
        this.getProducts();
      } catch (error) {
        console.error(error);
      }
    },
    cancelActions() {
      this.showAddProduct = false;
      this.newProduct = {
        name: '',
        description: '',
        price: 0,
      };
    }
  }
}
</script>
<style>
body {
  font-family: 'Roboto', sans-serif !important;
}
.title__container {
  margin: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  background-color: #f0f0f0;
  border-radius: 5px;
  font-size: 24px;
  font-family: 'Roboto', sans-serif;
  color: #8a8a8a;
}
table {
  width: 100%;
  border-collapse: collapse;
}
tbody {
  font-size: 14px;
}
td {
  padding: 10px;
  border: 1px solid #e0e0e0;
  text-align: center;
}
th {
  padding: 10px;
  border: 1px solid #e0e0e0;
}
button {
  padding: 10px;
  border: 1px solid #e0e0e0;
  border-radius: 10px;
  margin: 5px;
}
.delete__button {
  background-color: #e44242;
  color: #fff;
}
.add__product__container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
  padding: 10px;
  border: 1px solid #e0e0e0;
  border-radius: 10px;
}
.input__product {
  width: 100%;
  padding: 10px;
  border: 1px solid #e0e0e0;
  border-radius: 10px;
  margin: 5px;
}
</style>