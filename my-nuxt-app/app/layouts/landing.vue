<script setup lang="ts">
// no header logic here anymore
</script>

<template>
  <div class="landing-shell">
    <!-- Shared header component -->
    <AppHeader variant="landing" title="My Nuxt Website" />

    <main class="content-area">
      <slot />
    </main>
  </div>
</template>

<style scoped>
.landing-shell {
  min-height: 100vh;
}

.content-area {
  min-width: 0;
}

/* Landing-only: stack feature cards sooner */
@media (max-width: 820px) {
  :deep(.features .container) {
    grid-template-columns: 1fr;
  }
}
</style>
