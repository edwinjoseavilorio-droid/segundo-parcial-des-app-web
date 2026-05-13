<template>
  <div>
    <div class="d-flex justify-content-between align-items-center mb-4">
      <h2 class="fw-bold text-primary">
        <i class="fa-solid fa-flask me-2"></i>gestion de proyectos
      </h2>
      <button class="btn btn-primary" @click="abrirModalCrear">
        <i class="fa-solid fa-plus me-2"></i>Nuevo proyecto
      </button>
    </div>

    <!-- Tabla de productos -->
    <div class="card border-0 shadow-sm">
      <div class="card-body p-0">
        <table class="table table-hover mb-0">
          <thead class="table-primary">
            <tr>
              <th>#</th>
              <th>Imagen</th>
              <th>Nombre</th>
              <th>Descripción</th>
              <th>Precio</th>
              <th class="text-center">Acciones</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="producto in productos" :key="producto.id">
              <td>{{ producto.id }}</td>
              <td>
                <img :src="producto.image || placeholderImage" :alt="producto.name"
                  @error="handleImageError"
                  style="width: 60px; height: 60px; object-fit: cover; border-radius: 8px;">
              </td>
              <td>{{ producto.name }}</td>
              <td class="text-muted small">{{ producto.description }}</td>
              <td>{{ producto.price?.toLocaleString('es-CO', { style: 'currency', currency: 'COP' }) }}</td>
              <td class="text-center">
                <button class="btn btn-sm btn-outline-primary me-1" @click="abrirModalEditar(producto)">
                  <i class="fa-solid fa-pen"></i>
                </button>
                <button class="btn btn-sm btn-outline-danger" @click="eliminarProducto(producto.id)">
                  <i class="fa-solid fa-trash"></i>
                </button>
                <button class="btn btn-sm btn-outline-info ms-1" @click="verDetalle(producto)">
                  <i class="fa-solid fa-eye"></i>
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <!-- Modal crear/editar proyectos -->
    <div v-if="mostrarModal" class="modal-overlay" @click.self="cerrarModal">
      <div class="modal-box card shadow-lg p-4">
        <h5 class="fw-bold text-primary mb-3">
          <i class="fa-solid fa-flask me-2"></i>
          {{ modoEditar ? 'Editar Servicio' : 'Nuevo Servicio' }}
        </h5>

        <div class="mb-3">
          <label class="form-label fw-semibold">Nombre</label>
          <input v-model="form.name" type="text" class="form-control" placeholder="Nombre del servicio" />
        </div>

        <div class="mb-3">
          <label class="form-label fw-semibold">Descripción</label>
          <textarea v-model="form.description" class="form-control" rows="3"
            placeholder="Descripción del servicio"></textarea>
        </div>

        <div class="mb-3">
          <label class="form-label fw-semibold">Precio (COP)</label>
          <input v-model.number="form.price" type="number" class="form-control" placeholder="Ej: 120000 " />
        </div>

        <div class="mb-3">
          <label class="form-label fw-semibold">URL de imagen</label>
          <input v-model="form.image" type="text" class="form-control" placeholder="https://..." />
        </div>

        <div class="d-flex gap-2 justify-content-end">
          <button class="btn btn-secondary" @click="cerrarModal">Cancelar</button>
          <button class="btn btn-primary" @click="guardarProducto">
            <i class="fa-solid fa-floppy-disk me-1"></i>
            {{ modoEditar ? 'Guardar cambios' : 'Crear servicio' }}
          </button>
        </div>
      </div>
    </div>
    <!-- Modal ver detalle -->
    <div v-if="mostrarDetalle" class="modal-overlay" @click.self="mostrarDetalle = false">
      <div class="modal-box card shadow-lg p-4">
        <h5 class="fw-bold text-primary mb-3">
          <i class="fa-solid fa-eye me-2"></i>Detalle del Servicio
        </h5>
        <img :src="productoDetalle.image || placeholderImage" :alt="productoDetalle.name" class="w-100 mb-3"
          @error="handleImageError"
          style="height: 200px; object-fit: cover; border-radius: 8px;">
        <h6 class="fw-bold">{{ productoDetalle.name }}</h6>
        <p class="text-muted">{{ productoDetalle.description }}</p>
        <p class="fw-bold text-success">{{ productoDetalle.price?.toLocaleString('es-CO', { style: 'currency', currency: 'COP' }) }}</p>
        <button class="btn btn-secondary w-100" @click="mostrarDetalle = false">Cerrar</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import productosIniciales from '../data/products.json'
import placeholderImage from '../assets/placeholder.svg'

const productos = ref([])
const mostrarModal = ref(false)
const modoEditar = ref(false)
const form = ref({ id: null, name: '', description: '', price: 0, image: '' })

const mostrarDetalle = ref(false)
const productoDetalle = ref({})

function handleImageError(event) {
  event.target.src = placeholderImage
}

// Cargar productos desde LocalStorage o JSON inicial
onMounted(() => {
  const guardados = localStorage.getItem('productos')
  if (guardados) {
    productos.value = JSON.parse(guardados)
    const productosSync = [1, 2, 3, 10]
    let actualizo = false

    productosSync.forEach(id => {
      const imagenInicial = productosIniciales.find(p => p.id === id)?.image
      const productoLocal = productos.value.find(p => p.id === id)
      if (productoLocal && imagenInicial && productoLocal.image !== imagenInicial) {
        productoLocal.image = imagenInicial
        actualizo = true
      }
    })

    if (actualizo) guardarEnStorage()
  } else {
    productos.value = productosIniciales
    guardarEnStorage()
  }
})

function guardarEnStorage() {
  localStorage.setItem('productos', JSON.stringify(productos.value))
}

function abrirModalCrear() {
  modoEditar.value = false
  form.value = { id: null, name: '', description: '', price: 0, image: '' }
  mostrarModal.value = true
}

function abrirModalEditar(producto) {
  modoEditar.value = true
  form.value = { ...producto }
  mostrarModal.value = true
}

function cerrarModal() {
  mostrarModal.value = false
}

function guardarProducto() {
  if (modoEditar.value) {
    const index = productos.value.findIndex(p => p.id === form.value.id)
    if (index !== -1) productos.value[index] = { ...form.value }
  } else {
    const nuevoId = productos.value.length > 0 ? Math.max(...productos.value.map(p => p.id)) + 1 : 1
    productos.value.push({ ...form.value, id: nuevoId })
  }
  guardarEnStorage()
  cerrarModal()
}

function eliminarProducto(id) {
  if (confirm('¿Estás seguro de eliminar este servicio?')) {
    productos.value = productos.value.filter(p => p.id !== id)
    guardarEnStorage()
  }

}
function verDetalle(producto) {
  productoDetalle.value = producto
  mostrarDetalle.value = true
}
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
}

.modal-box {
  width: 100%;
  max-width: 520px;
  background: white;
  border-radius: 12px;
}
</style>