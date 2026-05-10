<template>
  <div class="card h-100 border-0 shadow-sm">
    <img :src="producto.image" :alt="producto.name"
         class="card-img-top" style="height: 180px; object-fit: cover;">
    <div class="card-body d-flex flex-column">
      <h6 class="fw-bold text-primary">{{ producto.name }}</h6>
      <p class="text-muted small flex-grow-1">{{ producto.description }}</p>
      <div class="d-flex justify-content-between align-items-center mt-2">
        <span class="fw-bold text-success">
          $ {{ producto.price.toLocaleString() }}
        </span>
        <button class="btn btn-sm btn-outline-primary"
          @click="$emit('ver-detalle', producto)">
          <i class="fa-solid fa-eye me-1"></i> Ver
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  producto: {
    type: Object,
    required: true
  }
})

defineEmits(['ver-detalle'])
</script>