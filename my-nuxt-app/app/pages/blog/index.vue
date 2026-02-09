<script setup lang="ts">
useHead({
  title: 'Blog',
  meta: [{ name: 'description', content: 'Posts and updates.' }]
})

const route = useRoute()
const router = useRouter()
const query = computed(() => route.query ?? {})

// Pull blog posts from the "blog" collection
const { data: posts } = await useAsyncData('blog-posts', () =>
  queryCollection('blog').all()
)

type Entry = {
  _path: string
  title?: string
  description?: string
  excerpt?: string
  tags?: string[]
  category?: string
  date?: string
  image?: string
}

const allPosts = computed<Entry[]>(() => (posts.value ?? []) as Entry[])

// URL state
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

// Filter logic
const filteredPosts = computed(() => {
  let result = allPosts.value

  if (selectedCategory.value) {
    result = result.filter(p => p.category === selectedCategory.value)
  }

  if (selectedTags.value.length) {
    result = result.filter(p =>
      selectedTags.value.every(tag => (p.tags ?? []).includes(tag))
    )
  }

  const q = searchQuery.value.trim().toLowerCase()
  if (q) {
    result = result.filter(p => {
      const haystack =
        `${p.title ?? ''} ${p.excerpt ?? ''} ${p.description ?? ''} ${(p.tags ?? []).join(' ')} ${p.category ?? ''}`.toLowerCase()
      return haystack.includes(q)
    })
  }

  // Optional: sort newest first if dates exist
  result = [...result].sort((a, b) => (b.date ?? '').localeCompare(a.date ?? ''))

  return result
})

// Chip actions
function clearAll() { router.push({ query: {} }) }
function clearCategory() {
  const next = { ...query.value }
  delete next.category
  router.push({ query: next })
}
function removeTag(tag: string) {
  const nextTags = selectedTags.value.filter(t => t !== tag)
  const next = { ...query.value }
  if (nextTags.length) next.tag = nextTags
  else delete next.tag
  router.push({ query: next })
}
function clearSearch() {
  const next = { ...query.value }
  delete next.q
  router.push({ query: next })
}
</script>

<template>
  <section>
    <div class="container">
      <h1>Blog</h1>
      <p>Posts and updates.</p>

      <div class="results-bar">
        <div class="summary">
          <strong>{{ filteredPosts.length }}</strong>
          <span>result<span v-if="filteredPosts.length !== 1">s</span></span>

          <span v-if="selectedCategory" class="muted">
            in <strong>{{ selectedCategory }}</strong>
          </span>

          <span v-if="selectedTags.length" class="muted">
            tagged <strong>{{ selectedTags.join(', ') }}</strong>
          </span>

          <span v-if="searchQuery" class="muted">
            for “<strong>{{ searchQuery }}</strong>”
          </span>
        </div>

        <div v-if="selectedCategory || selectedTags.length || searchQuery" class="chips">
          <button v-if="selectedCategory" class="chip" @click="clearCategory">
            Category: {{ selectedCategory }} ✕
          </button>

          <button v-for="t in selectedTags" :key="t" class="chip" @click="removeTag(t)">
            Tag: {{ t }} ✕
          </button>

          <button v-if="searchQuery" class="chip" @click="clearSearch">
            Search: {{ searchQuery }} ✕
          </button>

          <button class="chip clear" @click="clearAll">Clear all</button>
        </div>
      </div>

      <div class="grid">
        <PreviewCard
          v-for="p in filteredPosts"
          :to="p._path || p.path"
          :key="p._path || p.path"
          :title="p.title || '(Untitled)'"
          subtitle="Blog"
          :excerpt="p.excerpt || p.description || ''"
          :image="p.image"
          :tags="p.tags || []"
        />
      </div>

      <div v-if="filteredPosts.length === 0" class="empty">
        <h2>No results</h2>
        <p>Try removing a filter or using a different search term.</p>
      </div>
    </div>
  </section>
</template>

<style scoped>
.grid { margin-top: 1.5rem; display: grid; grid-template-columns: repeat(auto-fit, minmax(320px, 1fr)); gap: 1rem; }
.results-bar { margin-top: 1rem; padding: 0.75rem 0; border-top: 1px solid var(--border); border-bottom: 1px solid var(--border); }
.summary { display: flex; flex-wrap: wrap; gap: 0.5rem; align-items: baseline; }
.muted { opacity: 0.75; }
.chips { margin-top: 0.6rem; display: flex; flex-wrap: wrap; gap: 0.5rem; }
.chip { border: 1px solid var(--border); background: #f6f6f6; border-radius: 999px; padding: 0.35rem 0.6rem; cursor: pointer; font-size: 0.85rem; }
.chip:hover { background: #f0f0f0; }
.chip.clear { background: #fff; }
.empty { margin-top: 2rem; opacity: 0.9; }
</style>
