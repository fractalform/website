<script setup lang="ts">
const route = useRoute()
const { nav } = useSiteNav()

// Used for mobile (tap to open). Desktop uses hover.
const openKey = ref<string | null>(null)

function toggleMenu(key: string) {
  openKey.value = openKey.value === key ? null : key
}

function closeMenus() {
  openKey.value = null
}

// Close on navigation
watch(() => route.fullPath, closeMenus)

// Close on outside click
onMounted(() => {
  const onDocClick = (e: MouseEvent) => {
    const target = e.target as HTMLElement | null
    if (!target) return
    if (target.closest('.top-nav')) return
    closeMenus()
  }

  document.addEventListener('click', onDocClick)
  onBeforeUnmount(() => document.removeEventListener('click', onDocClick))
})
</script>

<template>
  <div class="landing-shell">
    <header class="header-area">
      <div class="header-inner">
        <div class="brand">
          <NuxtLink to="/" class="brand-link">My Nuxt Website</NuxtLink>
        </div>

        <nav class="top-nav" aria-label="Primary">
          <div v-for="item in nav" :key="item.to" class="nav-item">
            <!-- Parent with children -->
            <template v-if="item.children?.length">
              <!-- Desktop: hover opens via CSS. Mobile: tap opens via openKey -->
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
                <!-- Optional: include the parent page itself as first item -->
                <NuxtLink :to="item.to" class="link submenu-link" role="menuitem" @click="closeMenus">
                  {{ item.label }}
                </NuxtLink>

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
              <NuxtLink :to="item.to" class="link" @click="closeMenus">
                {{ item.label }}
              </NuxtLink>
            </template>
          </div>
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
  align-items: center; /* important: align pills consistently */
}

.nav-item {
  position: relative;
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

/* Dropdown */
.submenu {
  position: absolute;
  right: 0;
  top: calc(100% + 0.15rem); /* reduced gap helps hover */
  min-width: 220px;

  padding: 0.35rem;
  border-radius: 12px;

  background: var(--surface);
  border: 1px solid var(--border);
  box-shadow: var(--shadow-md);

  display: none;
  z-index: 60;
}

/* Desktop: hover opens dropdown */
@media (min-width: 901px) {
  .nav-item:hover .submenu {
    display: block;
  }

  /* hover bridge so you can move down into the menu */
  .nav-item::after {
    content: "";
    position: absolute;
    left: -8px;
    right: -8px;
    top: 100%;
    height: 14px;
  }
}

/* Mobile: tap opens dropdown */
.submenu.open {
  display: block;
}

.submenu-link {
  display: block;
  width: 100%;
  padding: 0.55rem 0.65rem;
}

.content-area {
  min-width: 0;
}
</style>
