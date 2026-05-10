<template>
  <div class="sidebar d-flex flex-column py-3">

    <p class="text-white-50 small px-3 mb-2 text-uppercase fw-bold">Menú</p>

    <router-link to="/dashboard" exact-active-class="active">
      <i class="fa-solid fa-house me-2"></i> Inicio
    </router-link>

    <router-link to="/dashboard/productos" active-class="active">
      <i class="fa-solid fa-flask me-2"></i> Servicios
    </router-link>

    <router-link to="/dashboard/clientes" active-class="active">
      <i class="fa-solid fa-users me-2"></i> Clientes
    </router-link>

    <router-link to="/dashboard/proyectos" active-class="active">
      <i class="fa-solid fa-briefcase me-2"></i> Proyectos
    </router-link>

    <router-link to="/dashboard/contacto" active-class="active">
      <i class="fa-solid fa-envelope me-2"></i> Contacto
    </router-link>

  </div>
</template>

<script setup>
</script>