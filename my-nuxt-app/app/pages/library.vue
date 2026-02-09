<script setup lang="ts">
useHead({
  title: 'Library',
  meta: [{ name: 'description', content: 'Browse all content on the site.' }]
})

const route = useRoute()
const router = useRouter()
const query = computed(() => route.query ?? {})

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

const searchQuery = computed(() => {
  const q = query.value.q
  return typeof q === 'string' ? q : ''
})

// Pull both collections
const { data: blog } = await useAsyncData('lib-blog', () => queryCollection('blog').all())
const { data: pages } = await useAsyncData('lib-pages', () => queryCollection('pages').all())

type Entry = {
  _path?: string
  path?: string
  to?: string
  title?: string
  description?: string
  excerpt?: string
  tags?: string[]
  category?: string
  date?: string
  image?: string
  kind: 'Blog' | 'Page'
}

const items = computed<Entry[]>(() => {
  const b = (blog.value ?? []).map((x: any) => ({
    ...x,
    kind: 'Blog' as const,
    to: x._path ?? x.path
  }))

  const p = (pages.value ?? []).map((x: any) => ({
    ...x,
    kind: 'Page' as const,
    to: x._path ?? x.path
  }))

  return [...b, ...p].filter(i => typeof i.to === 'string' && i.to.length > 0)
})

const allItems = computed(() => items.value)

const filteredItems = computed(() => {
  let result = allItems.value

  if (selectedCategory.value) {
    result = result.filter(i => i.category === selectedCategory.value)
  }

  if (selectedTags.value.length) {
    result = result.filter(i =>
      selectedTags.value.every(tag => (i.tags ?? []).includes(tag))
    )
  }

  const q = searchQuery.value.trim().toLowerCase()
  if (q) {
    result = result.filter(i => {
      const haystack =
        `${i.title ?? ''} ${i.excerpt ?? ''} ${i.description ?? ''} ${(i.tags ?? []).join(' ')} ${i.category ?? ''}`.toLowerCase()
      return haystack.includes(q)
    })
  }

  // nice default sort: newest first if date exists, otherwise title
  result = [...result].sort((a, b) => (b.date ?? '').localeCompare(a.date ?? '') || (a.title ?? '').localeCompare(b.title ?? ''))

  return result
})

function clearAll() { router.push({ query: {} }) }
</script>

<template>
  <section>
    <div class="container">
      <h1>Library</h1>
      <p>Everything on the site — pages and posts.</p>
      <p class="muted">{{ filteredItems.length }} result<span v-if="filteredItems.length !== 1">s</span></p>

      <div class="grid">
        <PreviewCard
            v-for="i in filteredItems"
            :key="i.to"
            :to="i.to"
            :title="i.title || '(Untitled)'"
            :subtitle="i.kind"
            :excerpt="i.excerpt || i.description || ''"
            :image="i.image"
            :tags="i.tags || []"
            />
      </div>

      <div v-if="filteredItems.length === 0" class="empty">
        <h2>No results</h2>
        <p>Try removing a filter or using a different search term.</p>
        <button class="chip clear" @click="clearAll">Clear all</button>
      </div>
    </div>
  </section>
</template>

<style scoped>
.grid { margin-top: 1.5rem; display: grid; grid-template-columns: repeat(auto-fit, minmax(320px, 1fr)); gap: 1rem; }
.empty { margin-top: 2rem; opacity: 0.9; }
.chip { border: 1px solid var(--border); background: #f6f6f6; border-radius: 999px; padding: 0.35rem 0.6rem; cursor: pointer; font-size: 0.85rem; }
.chip.clear { background: #fff; }
.muted { opacity: 0.75; }
</style>
