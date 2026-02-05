<template>
  <div class="feature">
    <h3>{{ title }}</h3>
    <p>{{ description }}</p>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  title: string
  description: string
}>()
</script>

<style scoped>
.feature {
  padding: 1.5rem;
  border: 1px solid #ddd;
  border-radius: 8px;
}
</style>