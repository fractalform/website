<script setup lang="ts">
import { navItems } from '~/data/nav'
import { posts } from '~/data/posts'

const route = useRoute()
const router = useRouter()

// Safe, always-an-object query view
const query = computed(() => route.query ?? {})

const categories = computed(() =>
  Array.from(new Set(posts.map(p => p.category))).sort()
)

const tags = computed(() =>
  Array.from(new Set(posts.flatMap(p => p.tags))).sort()
)

const selectedCategory = computed(() => {
  const c = query.value.category
  return typeof c === 'string' ? c : undefined
})

const selectedTags = computed<string[]>(() => {
  const t = query.value.tag
  const raw =
    Array.isArray(t) ? t :
    typeof t === 'string' ? [t] :
    []
  return raw.filter((x): x is string => typeof x === 'string' && x.length > 0)
})

const searchValue = computed(() => {
  const q = query.value.q
  return typeof q === 'string' ? q : ''
})

function pushQuery(next: Record<string, any>) {
  router.push({ query: next })
}

function setSearch(value: string) {
  const next = { ...query.value }
  const v = value.trim()

  if (v) next.q = v
  else delete next.q

  pushQuery(next)
}

function toggleTag(tag: string) {
  const nextTags = selectedTags.value.includes(tag)
    ? selectedTags.value.filter(t => t !== tag)
    : [...selectedTags.value, tag]

  const next = { ...query.value }

  if (nextTags.length) next.tag = nextTags
  else delete next.tag

  pushQuery(next)
}

function toggleCategory(cat: string) {
  const next = { ...query.value }

  // clicking the active category clears it
  if (selectedCategory.value === cat) delete next.category
  else next.category = cat

  pushQuery(next)
}

function clearFilters() {
  // optionally keep search; or clear everything:
  pushQuery({})
}
</script>

<template>
  <aside class="sidebar">
    <div class="sidebar-inner">
      <div class="group">
        <div class="group-title">Navigation</div>
            <nav class="nav">
            <NuxtLink v-for="i in navItems" :key="i.to" :to="i.to" class="link">
                {{ i.label }}
            </NuxtLink>
            </nav>
      </div>

    <div class="filter-section">
    <details open>
  <summary>Search</summary>

  <input
    class="search"
    type="text"
    placeholder="Search posts..."
    :value="searchValue"
    @input="setSearch(($event.target as HTMLInputElement).value)"
  />
</details>
    <details open>
        <summary>Categories</summary>

        <button
        v-for="cat in categories"
        :key="cat"
        class="filter-btn"
        :class="{ active: selectedCategory === cat }"
        @click="toggleCategory(cat)"
        >
        {{ cat }}
        </button>
    </details>

    <details open>
        <summary>Tags</summary>

        <button
        v-for="tag in tags"
        :key="tag"
        class="filter-btn"
        :class="{ active: selectedTags.includes(tag) }"
        @click="toggleTag(tag)"
        >
        {{ tag }}
        </button>
    </details>
    </div>
    </div>
  </aside>
</template>

<style scoped>
.sidebar-inner {
  padding: 1.25rem 1rem;
}

.group + .group {
  margin-top: 1.25rem;
}

.group-title {
  font-size: 0.9rem;
  font-weight: 600;
  opacity: 0.75;
  margin-bottom: 0.75rem;
}

.nav {
  display: grid;
  gap: 0.4rem;
}

.link {
  padding: 0.55rem 0.65rem;
  border-radius: 8px;
  text-decoration: none;
  color: inherit;
}

.link:hover {
  background: #f3f4f6;
}

.link.router-link-active {
  background: #eef2ff;
}

.pill-row {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}

.pill {
  border: 1px solid #e7e7e7;
  background: #fafafa;
  padding: 0.4rem 0.6rem;
  border-radius: 999px;
  font-size: 0.85rem;
  cursor: pointer;
}

.pill:hover {
  background: #f3f4f6;
}

.filter-section details {
  margin-bottom: 1rem;
}

.filter-section summary {
  cursor: pointer;
  font-size: 0.8rem;
  text-transform: uppercase;
  opacity: 0.6;
  margin-bottom: 0.4rem;
}

.filter-btn {
  display: block;
  width: 100%;
  padding: 0.4rem 0.6rem;
  text-align: left;
  border-radius: 8px;
  border: none;
  background: transparent;
  cursor: pointer;
}

.filter-btn:hover {
  background: rgba(0,0,0,0.05);
}

.filter-btn.active {
  background: rgba(0,0,0,0.1);
  font-weight: 600;
}

.tag-filter:hover {
  background: rgba(0,0,0,0.05);
}

.tag-filter.active {
  background: rgba(0,0,0,0.1);
  font-weight: 600;
}

.search {
  width: 100%;
  padding: 0.5rem 0.6rem;
  border-radius: 10px;
  border: 1px solid var(--border);
  background: #fff;
}
.search:focus {
  outline: 3px solid rgba(99, 102, 241, 0.2);
  border-color: rgba(99, 102, 241, 0.45);
}
</style>