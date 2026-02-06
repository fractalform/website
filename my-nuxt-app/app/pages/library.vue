<script setup lang="ts">
useHead({
  title: 'Library',
  meta: [{ name: 'description', content: 'Browse previews and content.' }]
})

const items = [
  {
    to: '/blog/first-post',
    title: 'First Post',
    subtitle: 'Blog',
    excerpt: 'A quick post to prove the pipeline: cards, tags, and navigation.',
    image: '/favicon.ico',
    tags: ['blog', 'nuxt']
  },
  {
    to: '/about',
    title: 'About this site',
    subtitle: 'Page',
    excerpt: 'What we’re building and why it’s structured this way.',
    image: '/images/AL-Symbol2a.png',
    tags: ['structure', 'design']
  },
  {
    to: '/',
    title: 'Other Page Layout',
    subtitle: 'Page',
    excerpt: 'An additional card that will help make sure all this is flowing nicely.',
    tags: ['testing', 'management']
  },
  {
    to: '/',
    title: 'Homepage Layout',
    subtitle: 'Page',
    excerpt: 'Hero sections, feature cards, and consistent container rhythm.',
    tags: ['layout', 'design']
  },
  {
    to: '/about',
    title: 'About This Project',
    subtitle: 'Page',
    excerpt: 'Why this site is structured as a system, not a pile of pages.',
    tags: ['architecture']
  }
]
</script>

<template>
  <section>
    <div class="container">
      <div class="header">
        <h1>Library</h1>
        <p>Browse pages and previews. This is our “Modrinth-style” directory foundation.</p>
      </div>

      <div class="grid">
        <PreviewCard
          v-for="i in items"
          :key="i.to"
          :to="i.to"
          :title="i.title"
          :subtitle="i.subtitle"
          :excerpt="i.excerpt"
          :image="i.image"
          :tags="i.tags"
        />
    </div>
    </div>
  </section>
</template>

<style scoped>
.header p {
  opacity: 0.85;
}

.grid {
  margin-top: 1.5rem;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  gap: 1rem;
}
</style>
