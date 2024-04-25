<script setup>
  const props = defineProps({
    nombre: String,
    tareaHecha: Boolean,
    posicion: Number
  })

</script>

<template>
    {{ nombre }} - {{ posicion }} {{ tareaHecha }}
</template>