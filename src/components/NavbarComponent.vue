<template>
  <nav class="navbar navbar-expand-lg shadow-sm" style="background-color: #034f84;">
    <div class="container-fluid">

      <a class="navbar-brand d-flex align-items-center gap-2" href="#">
        <img src="https://ingeocimyc.com.co/assets/logo-ingeocimyc.svg"
             alt="INGEOCIMYC" style="height: 40px; filter: brightness(0) invert(1);">
        <span class="text-white fw-bold">INGEOCIMYC</span>
      </a>

      <div class="d-flex align-items-center gap-3">
        <span class="text-white small">
          <i class="fa-solid fa-user me-1"></i>{{ nombreUsuario }}
        </span>
        <button @click="logout" class="btn btn-outline-light btn-sm">
          <i class="fa-solid fa-right-from-bracket me-1"></i> Salir
        </button>
      </div>

    </div>
  </nav>
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const nombreUsuario = ref(localStorage.getItem('nombreUsuario') || 'Usuario')

function logout() {
  localStorage.removeItem('loggedIn')
  localStorage.removeItem('nombreUsuario')
  router.push('/login')
}
</script>