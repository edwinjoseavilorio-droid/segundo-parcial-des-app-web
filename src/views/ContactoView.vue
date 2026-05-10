<template>
  <div>
    <h2 class="fw-bold text-primary mb-4">
      <i class="fa-solid fa-envelope me-2"></i>Contacto
    </h2>

    <div class="row g-4">
      <div class="col-md-6">
        <div class="card border-0 shadow-sm p-4">
          <h5 class="fw-bold mb-3">Envíanos un mensaje</h5>

          <div class="mb-3">
            <label class="form-label fw-semibold">Nombre</label>
            <input v-model="form.nombre" type="text" class="form-control"
              placeholder="Tu nombre completo" />
          </div>

          <div class="mb-3">
            <label class="form-label fw-semibold">Correo electrónico</label>
            <input v-model="form.email" type="email" class="form-control"
              placeholder="correo@ejemplo.com" />
          </div>

          <div class="mb-3">
            <label class="form-label fw-semibold">Mensaje</label>
            <textarea v-model="form.mensaje" class="form-control" rows="4"
              placeholder="¿En qué podemos ayudarte?"></textarea>
          </div>

          <button class="btn btn-primary w-100" @click="enviarMensaje">
            <i class="fa-solid fa-paper-plane me-2"></i>Enviar mensaje
          </button>

          <div v-if="enviado" class="alert alert-success mt-3">
            <i class="fa-solid fa-circle-check me-2"></i>
            ¡Mensaje enviado correctamente!
          </div>
        </div>
      </div>

      <div class="col-md-6">
        <div class="card border-0 shadow-sm p-4 h-100">
          <h5 class="fw-bold mb-3">Información de contacto</h5>
          <ul class="list-unstyled">
            <li class="mb-3">
              <i class="fa-solid fa-location-dot text-primary me-2"></i>
              Bogotá, Colombia
            </li>
            <li class="mb-3">
              <i class="fa-solid fa-phone text-primary me-2"></i>
              +57 301 351 7044
            </li>
            <li class="mb-3">
              <i class="fa-solid fa-envelope text-primary me-2"></i>
              info@ingeocimyc.com.co
            </li>
            <li class="mb-3">
              <i class="fa-brands fa-whatsapp text-success me-2"></i>
              WhatsApp: 301 351 7044
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const enviado = ref(false)
const form = ref({ nombre: '', email: '', mensaje: '' })

function enviarMensaje() {
  if (form.value.nombre && form.value.email && form.value.mensaje) {
    enviado.value = true
    form.value = { nombre: '', email: '', mensaje: '' }
    setTimeout(() => { enviado.value = false }, 3000)
  }
}
</script>