<script setup lang="ts">
import { navItems } from '~/data/nav'
</script>

<template>
  <div class="landing-shell">
    <header class="header-area">
      <div class="header-inner">
        <div class="brand">
          <NuxtLink to="/" class="brand-link">My Nuxt Website</NuxtLink>
        </div>
        <nav class="top-nav">
          <NuxtLink v-for="i in navItems" :key="i.to" :to="i.to" class="link">
            {{ i.label }}
          </NuxtLink>
        </nav>
      </div>
    </header>

    <main class="content-area">
      <slot />
    </main>
  </div>
</template>

<style scoped>
.landing-shell {
  min-height: 100vh;
}

/* same header styling as default.vue */
.header-area {
  position: sticky;
  top: 0;
  z-index: 10;

  background: rgba(255, 255, 255, 0.82);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);

  border-bottom: 1px solid var(--border);
}

.header-inner {
  height: var(--header-h);
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 var(--gutter);
  max-width: var(--container);
  margin: 0 auto;
}

.brand-link {
  color: var(--text);
  text-decoration: none;
  font-weight: 700;
  letter-spacing: -0.01em;
}

.top-nav {
  display: flex;
  gap: 0.35rem;
  flex-wrap: wrap;
  justify-content: flex-end;
}

.link {
  color: var(--text);
  text-decoration: none;
  opacity: 0.86;

  padding: 0.45rem 0.65rem;
  border-radius: 10px;
  transition: background 120ms ease, opacity 120ms ease;
}

.link:hover {
  opacity: 1;
  background: rgba(0, 0, 0, 0.05);
}

.link.router-link-active {
  opacity: 1;
  background: rgba(99, 102, 241, 0.12);
}

.content-area {
  min-width: 0;
}
</style>
