<template>
  <div class="login-bg d-flex align-items-center justify-content-center min-vh-100">
    <div class="card shadow-lg p-4" style="max-width: 420px; width: 100%;">

      <div class="text-center mb-4">
        <img src="https://ingeocimyc.com.co/assets/logo-ingeocimyc.svg"
             alt="INGEOCIMYC" style="height: 60px;" class="mb-3">
        <h4 class="fw-bold text-primary">Iniciar Sesión</h4>
        <p class="text-muted small">Sistema de Gestión — INGEOCIMYC</p>
      </div>

      <form @submit.prevent="handleLogin">
        <div class="mb-3">
          <label class="form-label fw-semibold">
            <i class="fa-solid fa-user me-1"></i> Usuario
          </label>
          <input
            v-model="username"
            type="text"
            class="form-control"
            placeholder="Ingresa tu usuario"
            required
            autofocus
          />
        </div>

        <div class="mb-3">
          <label class="form-label fw-semibold">
            <i class="fa-solid fa-lock me-1"></i> Contraseña
          </label>
          <input
            v-model="password"
            type="password"
            class="form-control"
            placeholder="Ingresa tu contraseña"
            required
          />
        </div>

        <button type="submit" class="btn btn-primary w-100 mt-2">
          <i class="fa-solid fa-right-to-bracket me-2"></i> Entrar
        </button>

        <div v-if="errorMsg" class="alert alert-danger mt-3 py-2 small">
          <i class="fa-solid fa-circle-exclamation me-1"></i> {{ errorMsg }}
        </div>

        <div v-if="successMsg" class="alert alert-success mt-3 py-2 small">
          <i class="fa-solid fa-circle-check me-1"></i> {{ successMsg }}
        </div>
      </form>

      <div class="alert alert-info mt-4 small">
        <strong>👤 Demo:</strong> admin &nbsp;|&nbsp;
        <strong>🔑 Contraseña:</strong> 1234<br>
        <span class="text-muted">⚠️ Solo con fines educativos.</span>
      </div>

    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import usuarios from '../data/usuarios.json'

const router = useRouter()
const username = ref('')
const password = ref('')
const errorMsg = ref('')
const successMsg = ref('')

function handleLogin() {
  errorMsg.value = ''
  successMsg.value = ''

  const user = usuarios.find(
    u => u.username === username.value && u.password === password.value
  )

  if (user) {
    localStorage.setItem('loggedIn', 'true')
    localStorage.setItem('nombreUsuario', user.nombre)
    successMsg.value = '✅ Acceso correcto. Redirigiendo...'
    setTimeout(() => {
      router.push('/dashboard')
    }, 800)
  } else {
    errorMsg.value = 'Usuario o contraseña incorrectos'
    username.value = ''
    password.value = ''
  }
}
</script>

<style scoped>
.login-bg {
  background: linear-gradient(135deg, #034f84 0%, #0077b6 100%);
}
</style>