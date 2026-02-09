<script setup lang="ts">
const props = defineProps<{
  to?: string
}>()
</script>

<template>
  <NuxtLink v-if="props.to" :to="props.to" class="card">
    <slot />
  </NuxtLink>

  <div v-else class="card">
    <slot />
  </div>
</template>

<style scoped>
.card {
  display: block;
  color: inherit;
  text-decoration: none;
}
</style>
