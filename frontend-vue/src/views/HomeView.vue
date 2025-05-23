<template>
  <div class="min-h-screen bg-gray-50 py-8">
    <div class="max-w-2xl mx-auto px-4">
      <h1 class="text-3xl font-bold text-gray-900 mb-8">Gestión de Productos</h1>
      
      <form @submit.prevent="handleSubmit" class="bg-white p-6 rounded-lg shadow-sm mb-8">
        <div class="space-y-4">
          <div>
            <label htmlFor="name" className="block text-sm font-medium text-gray-700">Nombre</label>
            <input
            type="text"
            v-model="newProduct.name"
            placeholder="Nombre del producto"
            class="w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
            required
          />
          </div>
          
          <div>
            <label htmlFor="price" className="block text-sm font-medium text-gray-700">Precio</label>
          <input
            type="number"
            v-model="newProduct.price"
            placeholder="Precio"
            class="w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
            required
          />
        </div>
          <button
            type="submit"
            class="w-full bg-blue-600 text-white py-2 px-4 rounded-md hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 transition-colors"
          >
            Agregar Producto
          </button>
        </div>
      </form>

      <div class="bg-white rounded-lg shadow-sm">
        <ul class="divide-y divide-gray-200">
          <li v-for="product in products" :key="product.id" class="p-6 flex items-center justify-between">
            <div>
              <h3 class="text-lg font-medium text-gray-900">{{ product.name }}</h3>
              <p class="text-sm text-gray-500">${{ product.price }}</p>
            </div>
            <button
              @click="handleDelete(product.id)"
              class="text-red-600 hover:text-red-800 focus:outline-none focus:ring-2 focus:ring-red-500 focus:ring-offset-2 transition-colors"
            >
              Eliminar
            </button>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'HomeView',
  data() {
    return {
      products: [],
      newProduct: {
        name: '',
        price: ''
      }
    }
  },
  methods: {
    async fetchProducts() {
      try {
        const response = await axios.get('http://localhost:8000/api/products')
        this.products = response.data
      } catch (error) {
        console.error('Error fetching products:', error)
      }
    },
    async handleSubmit() {
      try {
        await axios.post('http://localhost:8000/api/products', this.newProduct)
        this.newProduct = { name: '', price: '' }
        this.fetchProducts()
      } catch (error) {
        console.error('Error creating product:', error)
      }
    },
    async handleDelete(id) {
      try {
        await axios.delete(`http://localhost:8000/api/products/${id}`)
        this.fetchProducts()
      } catch (error) {
        console.error('Error deleting product:', error)
      }
    }
  },
  mounted() {
    this.fetchProducts()
  }
}
</script>
