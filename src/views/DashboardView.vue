<template>
  <div class="d-flex flex-column min-vh-100">

    <!-- Navbar superior -->
    <NavbarComponent />

    <div class="d-flex flex-grow-1">

      <!-- Sidebar izquierdo -->
      <SidebarComponent />

      <!-- Contenido principal -->
      <div class="main-content">

        <!-- Vista hija (productos, clientes, etc.) -->
        <router-view />

        <!-- Inicio del dashboard si no hay ruta hija activa -->
        <div v-if="$route.path === '/dashboard'">

          <h2 class="fw-bold text-primary mb-1">
            <i class="fa-solid fa-gauge me-2"></i>Panel de Control
          </h2>
          <p class="text-muted mb-4">Bienvenido, {{ nombreUsuario }} 👋</p>

          <!-- Tarjetas de resumen -->
          <div class="row g-3 mb-4">

            <div class="col-md-4">
              <div class="card border-0 shadow-sm">
                <div class="card-body d-flex align-items-center gap-3">
                  <div class="rounded-circle p-3" style="background:#e3f2fd;">
                    <i class="fa-solid fa-flask fa-lg text-primary"></i>
                  </div>
                  <div>
                    <div class="fw-bold fs-4">10</div>
                    <div class="text-muted small">Servicios registrados</div>
                  </div>
                </div>
              </div>
            </div>

            <div class="col-md-4">
              <div class="card border-0 shadow-sm">
                <div class="card-body d-flex align-items-center gap-3">
                  <div class="rounded-circle p-3" style="background:#e8f5e9;">
                    <i class="fa-solid fa-users fa-lg text-success"></i>
                  </div>
                  <div>
                    <div class="fw-bold fs-4">24</div>
                    <div class="text-muted small">Clientes activos</div>
                  </div>
                </div>
              </div>
            </div>

            <div class="col-md-4">
              <div class="card border-0 shadow-sm">
                <div class="card-body d-flex align-items-center gap-3">
                  <div class="rounded-circle p-3" style="background:#fff3e0;">
                    <i class="fa-solid fa-briefcase fa-lg text-warning"></i>
                  </div>
                  <div>
                    <div class="fw-bold fs-4">8</div>
                    <div class="text-muted small">Proyectos en curso</div>
                  </div>
                </div>
              </div>
            </div>

          </div>

          <!-- Tabla de servicios recientes -->
          <div class="card border-0 shadow-sm">
            <div class="card-header bg-primary text-white fw-bold">
              <i class="fa-solid fa-clock-rotate-left me-2"></i>Servicios Recientes
            </div>
            <div class="card-body p-0">
              <table class="table table-hover mb-0">
                <thead class="table-light">
                  <tr>
                    <th>Servicio</th>
                    <th>Precio</th>
                    <th>Estado</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="p in productosRecientes" :key="p.id">
                    <td>{{ p.name }}</td>
                    <td>$ {{ p.price.toLocaleString() }}</td>
                    <td>
                      <span class="badge bg-success">Activo</span>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>

        </div>
      </div>
    </div>

    <!-- Footer -->
    <FooterComponent />

  </div>
</template>

<script setup>
import { ref } from 'vue'
import NavbarComponent from '../components/NavbarComponent.vue'
import SidebarComponent from '../components/SidebarComponent.vue'
import FooterComponent from '../components/FooterComponent.vue'
import productos from '../data/products.json'

const nombreUsuario = ref(localStorage.getItem('nombreUsuario') || 'Usuario')
const productosRecientes = productos.slice(0, 5)
</script>