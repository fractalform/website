<script setup lang="ts">
import { posts } from '~/data/posts'

useHead({
  title: 'Blog',
  meta: [{ name: 'description', content: 'Posts and updates.' }]
})

const route = useRoute()
const router = useRouter()

const selectedTag = computed(() => route.query.tag as string | undefined)

const allTags = computed(() =>
  Array.from(new Set(posts.flatMap(p => p.tags))).sort()
)

const filteredPosts = computed(() => {
  if (!selectedTag.value) return posts
  return posts.filter(p => p.tags.includes(selectedTag.value!))
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