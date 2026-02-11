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

/* Header: app-like, crisp */
.header-area {
  grid-area: header;
  position: sticky;
  top: 0;
  z-index: 10;

  background: rgba(255, 255, 255, 0.82);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);

  border-bottom: 1px solid var(--border);
}

/* Align header to sidebar/content columns */
.header-inner {
  height: var(--header-h);
  display: grid;
  grid-template-columns: 280px 1fr;
  align-items: center;
}

/* left cell aligns with sidebar text */
.brand {
  padding-left: var(--gutter);
}

.brand-link {
  color: var(--text);
  text-decoration: none;
  font-weight: 700;
  letter-spacing: -0.01em;
}

/* right cell aligns with content column */
.top-nav {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  padding-right: var(--gutter);
  gap: 0.35rem;
}

/* Top nav items: pill hover like modern apps */
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

/* Sidebar: matches token system */
.sidebar-area {
  grid-area: sidebar;
  position: sticky;
  top: var(--header-h);
  height: calc(100vh - var(--header-h));
  overflow: auto;

  background: var(--surface);
  border-right: 1px solid var(--border);
}

.content-area {
  grid-area: content;
  min-width: 0;
}

/* Mobile: hide sidebar, header aligns to container */
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
    padding: 0 var(--gutter);
  }

  .brand {
    padding-left: 0;
  }

  .top-nav {
    justify-content: flex-start;
    padding-right: 0;
    margin-top: 0.35rem;
    gap: 0.5rem;
    flex-wrap: wrap;
  }
}

</style>
