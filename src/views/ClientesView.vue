<template>
  <div>
    <h2 class="fw-bold text-primary mb-4">
      <i class="fa-solid fa-users me-2"></i>Clientes
    </h2>

    <div class="card border-0 shadow-sm">
      <div class="card-body p-0">
        <table class="table table-hover mb-0">
          <thead class="table-primary">
            <tr>
              <th>#</th>
              <th>Nombre</th>
              <th>Empresa</th>
              <th>Teléfono</th>
              <th>Estado</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="cliente in clientes" :key="cliente.id">
              <td>{{ cliente.id }}</td>
              <td>{{ cliente.nombre }}</td>
              <td>{{ cliente.empresa }}</td>
              <td>{{ cliente.telefono }}</td>
              <td>
                <span class="badge bg-success">Activo</span>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const clientes = ref([
  { id: 1, nombre: 'Juan Pérez', empresa: 'Constructora ABC', telefono: '3001234567' },
  { id: 2, nombre: 'María López', empresa: 'Ingeniería XYZ', telefono: '3109876543' },
  { id: 3, nombre: 'Carlos Ruiz', empresa: 'Pavimentos SA', telefono: '3205551234' },
  { id: 4, nombre: 'Ana Gómez', empresa: 'Obras Civiles Ltda', telefono: '3154447890' },
  { id: 5, nombre: 'Luis Torres', empresa: 'Vías y Concretos', telefono: '3001112233' },
])
</script>