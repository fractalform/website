<script setup lang="ts">
import { posts } from '~/data/posts'

useHead({
  title: 'Blog',
  meta: [{ name: 'description', content: 'Posts and updates.' }]
})

const route = useRoute()

const selectedCategory = computed(
  () => route.query.category as string | undefined
)

const selectedTags = computed(() => {
  const q = route.query.tag
  if (!q) return []
  return Array.isArray(q) ? q : [q]
})

const filteredPosts = computed(() => {
  let result = posts

  if (selectedCategory.value) {
    result = result.filter(p => p.category === selectedCategory.value)
  }

  if (selectedTags.value.length) {
    result = result.filter(p =>
      selectedTags.value.every(tag => p.tags.includes(tag))
    )
  }

  return result
})
</script>

<template>
  <section>
    <div class="container">
      <h1>Blog</h1>
      <p>Posts and updates.</p>

      <div class="grid">
        <PreviewCard
            v-for="p in filteredPosts"
            :key="p.slug"
            :to="`/blog/${p.slug}`"
            :title="p.title"
            subtitle="Blog"
            :excerpt="p.excerpt"
            :image="p.image"
            :tags="p.tags"
        />
      </div>
    </div>
  </section>
</template>

<style scoped>
.grid {
  margin-top: 1.5rem;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  gap: 1rem;
}
</style>