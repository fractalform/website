<template>
  <component :is="as" :to="to" class="card">
    <slot />
  </component>
</template>

<script setup lang="ts">
const props = defineProps<{
  to?: string
}>()

const as = computed(() => (props.to ? resolveComponent('NuxtLink') : 'div'))
</script>

<style scoped>
.card {
  border-radius: var(--radius);
  border: 1px solid var(--border);
  background: var(--surface);
  padding: 1rem;
  text-decoration: none;
  color: inherit;

  transition: transform 120ms ease, box-shadow 120ms ease, border-color 120ms ease;
}

.card:hover {
  transform: translateY(-2px);
  border-color: #d6d6d6;
  box-shadow: 0 10px 24px rgba(0, 0, 0, 0.08);
}

.card:focus-visible {
  outline: 3px solid rgba(99, 102, 241, 0.35);
  outline-offset: 2px;
}
</style>