<script setup lang="ts">
const route = useRoute()
const { nav } = useSiteNav()

// Which parent menu is open (by `to`)
const openKey = ref<string | null>(null)

function toggleMenu(key: string) {
  openKey.value = openKey.value === key ? null : key
}

function closeMenus() {
  openKey.value = null
}

// Close dropdown when navigating
watch(
  () => route.fullPath,
  () => closeMenus()
)

// Close dropdown when clicking outside the header nav
onMounted(() => {
  const onDocClick = (e: MouseEvent) => {
    const target = e.target as HTMLElement | null
    if (!target) return

    // If click is inside the header nav, ignore
    if (target.closest('.header-nav')) return
    closeMenus()
  }

  document.addEventListener('click', onDocClick)
  onBeforeUnmount(() => {
    document.removeEventListener('click', onDocClick)
  })
})
</script>

<template>
  <div class="app-shell">
    <header class="header-area">
      <div class="header-inner">
        <div class="brand">
          <NuxtLink to="/" class="brand-link">My Nuxt Website</NuxtLink>
        </div>

        <!-- header nav -->
        <nav class="nav header-nav" aria-label="Primary">
          <div
            v-for="item in nav"
            :key="item.to"
            class="nav-item"
          >
            <!-- Parent with children -->
            <template v-if="item.children?.length">
              <button
                type="button"
                class="link menu-trigger"
                :aria-expanded="openKey === item.to"
                :aria-controls="`menu-${item.to}`"
                @click.stop="toggleMenu(item.to)"
              >
                {{ item.label }}
                <span class="chev" aria-hidden="true">▾</span>
              </button>

                <div
                  class="submenu"
                  :class="{ open: openKey === item.to }"
                  :id="`menu-${item.to}`"
                  role="menu"
                >
                <NuxtLink
                  v-for="child in item.children"
                  :key="child.to"
                  :to="child.to"
                  class="link submenu-link"
                  role="menuitem"
                  @click="closeMenus"
                >
                  {{ child.label }}
                </NuxtLink>
              </div>
            </template>

            <!-- Simple link -->
            <template v-else>
              <NuxtLink
                :to="item.to"
                class="link"
                @click="closeMenus"
              >
                {{ item.label }}
              </NuxtLink>
            </template>
          </div>
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
.nav {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  padding-right: var(--gutter);
  gap: 0.35rem;
}

/* Each top-level item can host a dropdown */
.nav-item {
  position: relative;
}

/* Hover bridge: prevents dropdown from disappearing when moving the mouse */
@media (min-width: 901px) {
  .nav-item {
    position: relative;
  }

  .nav-item::after {
    content: "";
    position: absolute;
    left: -6px;
    right: -6px;
    top: 100%;
    height: 14px; /* the “bridge” */
  }

  /* Ensure the dropdown sits above the bridge */
  .submenu {
    z-index: 60;
  }
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

/* Button should visually match links */
.menu-trigger {
  border: 0;
  margin: 0;
  background: transparent;
  cursor: pointer;
  display: inline-flex;
  align-items: center;
  line-height: 1;      /* important: consistent baseline */
  vertical-align: middle;
  gap: 0.35rem;
}

.chev {
  font-size: 0.85em;
  opacity: 0.75;
}

/* Dropdown (default hidden) */
.submenu {
  position: absolute;
  right: 0;
  top: calc(100% + 0.15rem);

  min-width: 220px;
  padding: 0.35rem;
  border-radius: 12px;

  background: var(--surface);
  border: 1px solid var(--border);
  box-shadow: var(--shadow-md);

  display: none; /* key change */
}

/* Desktop: hover opens dropdown */
@media (min-width: 901px) {
  .nav-item:hover .submenu {
    display: block;
  }
}

/* Mobile: click opens dropdown */
.submenu.open {
  display: block;
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

  .nav {
    justify-content: flex-start;
    padding-right: 0;
    margin-top: 0.35rem;
    gap: 0.5rem;
    flex-wrap: wrap;
  }

  .submenu {
    left: 0;
    right: auto;
  }
}
</style>
