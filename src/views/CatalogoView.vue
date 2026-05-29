<template>
  <div>
    <h1>Catálogo</h1>

    <input
      v-model="busqueda"
      placeholder="Buscar producto"
    />

    <div
      v-for="producto in productosFiltrados"
      :key="producto.id"
    >
      <h3>{{ producto.nombre }}</h3>

      <router-link :to="`/catalogo/${producto.id}`">
        Ver detalle
      </router-link>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { getProductos } from '@/services/productoService'

const productos = ref([])

const busqueda = ref('')

const productosFiltrados = computed(() =>
  productos.value.filter(p =>
    p.nombre
      .toLowerCase()
      .includes(busqueda.value.toLowerCase())
  )
)

onMounted(async () => {
  const response = await getProductos()

  productos.value = response.data
})
</script>