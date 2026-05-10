<template>
  <div>
    <div class="d-flex justify-content-between align-items-center mb-4">
      <h2 class="fw-bold text-primary">
        <i class="fa-solid fa-flask me-2"></i>gestion de productos
      </h2>
      <button class="btn btn-primary" @click="abrirModalCrear">
        <i class="fa-solid fa-plus me-2"></i>Nuevo Servicio
      </button>
    </div>

    <!-- Tabla de productos -->
    <div class="card border-0 shadow-sm">
      <div class="card-body p-0">
        <table class="table table-hover mb-0">
          <thead class="table-primary">
            <tr>
              <th>#</th>
              <th>Nombre</th>
              <th>Descripción</th>
              <th>Precio</th>
              <th class="text-center">Acciones</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="producto in productos" :key="producto.id">
              <td>{{ producto.id }}</td>
              <td>{{ producto.name }}</td>
              <td class="text-muted small">{{ producto.description }}</td>
              <td>$ {{ producto.price.toLocaleString() }}</td>
              <td class="text-center">
                <button class="btn btn-sm btn-outline-primary me-1"
                  @click="abrirModalEditar(producto)">
                  <i class="fa-solid fa-pen"></i>
                </button>
                <button class="btn btn-sm btn-outline-danger"
                  @click="eliminarProducto(producto.id)">
                  <i class="fa-solid fa-trash"></i>
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <!-- Modal crear/editar -->
    <div v-if="mostrarModal" class="modal-overlay" @click.self="cerrarModal">
      <div class="modal-box card shadow-lg p-4">
        <h5 class="fw-bold text-primary mb-3">
          <i class="fa-solid fa-flask me-2"></i>
          {{ modoEditar ? 'Editar Servicio' : 'Nuevo Servicio' }}
        </h5>

        <div class="mb-3">
          <label class="form-label fw-semibold">Nombre</label>
          <input v-model="form.name" type="text" class="form-control"
            placeholder="Nombre del servicio" />
        </div>

        <div class="mb-3">
          <label class="form-label fw-semibold">Descripción</label>
          <textarea v-model="form.description" class="form-control" rows="3"
            placeholder="Descripción del servicio"></textarea>
        </div>

        <div class="mb-3">
          <label class="form-label fw-semibold">Precio (COP)</label>
          <input v-model.number="form.price" type="number" class="form-control"
            placeholder="Ej: 120000" />
        </div>

        <div class="mb-3">
          <label class="form-label fw-semibold">URL de imagen</label>
          <input v-model="form.image" type="text" class="form-control"
            placeholder="https://..." />
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

  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import productosIniciales from '../data/products.json'

const productos = ref([])
const mostrarModal = ref(false)
const modoEditar = ref(false)
const form = ref({ id: null, name: '', description: '', price: 0, image: '' })

// Cargar productos desde LocalStorage o JSON inicial
onMounted(() => {
  const guardados = localStorage.getItem('productos')
  if (guardados) {
    productos.value = JSON.parse(guardados)
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
    const nuevoId = Math.max(...productos.value.map(p => p.id)) + 1
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
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.5);
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