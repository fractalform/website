<template>
  <AppCard>
    <h3>{{ title }}</h3>
    <p>{{ description }}</p>
  </AppCard>
</template>

<script setup lang="ts">
defineProps<{
  title: string
  description: string
}>()
</script>