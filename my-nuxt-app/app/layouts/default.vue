<script setup lang="ts">
import { navItems } from '~/data/nav'
</script>
<template>
  <div class="app-shell">
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

    <aside class="sidebar-area">
      <SideBar />
    </aside>

    <main class="content-area">
      <slot />
    </main>
  </div>
</template>

<style scoped>
.app-shell {
  min-height: 100vh;
  display: grid;
  grid-template-columns: 280px 1fr;
  grid-template-rows: var(--header-h) 1fr;
  grid-template-areas:
    "header header"
    "sidebar content";
}

.header-area {
  grid-area: header;
  position: sticky;
  top: 0;
  z-index: 10;
  background: #222;
  color: white;
  border-bottom: 1px solid rgba(255,255,255,0.08);
}

/* THIS is the key: header aligns to the same columns as the shell */
.header-inner {
  height: var(--header-h);
  display: grid;
  grid-template-columns: 280px 1fr;
  align-items: center;
}

/* left cell aligns with sidebar text */
.brand {
  padding-left: 1.5rem; /* matches your container side padding */
}

.brand-link {
  color: white;
  text-decoration: none;
  font-weight: 600;
}

/* right cell aligns with content column */
.top-nav {
  display: flex;
  justify-content: flex-end;
  padding-right: 1.5rem; /* matches container side padding */
}

.link {
  margin-left: 1rem;
  color: white;
  text-decoration: none;
  opacity: 0.9;
}

.link:hover {
  opacity: 1;
}

.link.router-link-active {
  text-decoration: underline;
}

.sidebar-area {
  grid-area: sidebar;
  position: sticky;
  top: var(--header-h);
  height: calc(100vh - var(--header-h));
  overflow: auto;
  background: #fff;
  border-right: 1px solid #e7e7e7;
}

.content-area {
  grid-area: content;
  min-width: 0;
}

/* Mobile: hide sidebar, make header a single column and align to content container */
@media (max-width: 900px) {
  .app-shell {
    grid-template-columns: 1fr;
    grid-template-areas:
      "header"
      "content";
  }

  .sidebar-area {
    display: none;
  }

  .header-inner {
    grid-template-columns: 1fr;
    padding: 0 1.5rem; /* same as container padding */
  }

  .brand {
    padding-left: 0;
  }

  .top-nav {
    justify-content: flex-start;
    padding-right: 0;
    margin-top: 0.35rem;
    gap: 1rem;
    flex-wrap: wrap;
  }

  .link {
    margin-left: 0;
  }
}
</style>
