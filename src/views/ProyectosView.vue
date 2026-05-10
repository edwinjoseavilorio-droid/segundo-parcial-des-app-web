<template>
  <div>
    <h2 class="fw-bold text-primary mb-4">
      <i class="fa-solid fa-briefcase me-2"></i>Proyectos
    </h2>

    <div class="row g-3">
      <div class="col-md-6" v-for="proyecto in proyectos" :key="proyecto.id">
        <div class="card border-0 shadow-sm h-100">
          <div class="card-body">
            <div class="d-flex justify-content-between align-items-start">
              <h6 class="fw-bold text-primary">{{ proyecto.nombre }}</h6>
              <span :class="'badge ' + proyecto.badgeColor">{{ proyecto.estado }}</span>
            </div>
            <p class="text-muted small mb-2">{{ proyecto.descripcion }}</p>
            <div class="d-flex gap-3 small text-muted">
              <span><i class="fa-solid fa-user me-1"></i>{{ proyecto.cliente }}</span>
              <span><i class="fa-solid fa-calendar me-1"></i>{{ proyecto.fecha }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const proyectos = ref([
  { id: 1, nombre: 'Pavimentación Vía Norte', descripcion: 'Control de mezclas asfálticas para vía principal.', cliente: 'Vías y Concretos', fecha: '2026-03-15', estado: 'En curso', badgeColor: 'bg-warning text-dark' },
  { id: 2, nombre: 'Edificio Torre Centro', descripcion: 'Ensayos de suelos y control de concreto en obra.', cliente: 'Constructora ABC', fecha: '2026-02-10', estado: 'Completado', badgeColor: 'bg-success' },
  { id: 3, nombre: 'Puente Vehicular Sur', descripcion: 'Análisis granulométrico y ensayos CBR.', cliente: 'Obras Civiles Ltda', fecha: '2026-04-01', estado: 'En curso', badgeColor: 'bg-warning text-dark' },
  { id: 4, nombre: 'Urbanización Los Pinos', descripcion: 'Estudio geotécnico completo para cimentaciones.', cliente: 'Ingeniería XYZ', fecha: '2026-01-20', estado: 'Completado', badgeColor: 'bg-success' },
  { id: 5, nombre: 'Ampliación Terminal', descripcion: 'Control de calidad de concreto en losas.', cliente: 'Pavimentos SA', fecha: '2026-05-05', estado: 'Pendiente', badgeColor: 'bg-secondary' },
  { id: 6, nombre: 'Vía Rural Km 12', descripcion: 'Ensayos de Proctor y CBR para subrasante.', cliente: 'Vías y Concretos', fecha: '2026-05-20', estado: 'Pendiente', badgeColor: 'bg-secondary' },
])
</script>