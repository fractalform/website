<script setup lang="ts">
const route = useRoute()

const { data: page, pending } = useAsyncData(
  () => `page:${route.path}`,
  () => queryCollection('pages').path(route.path).first(),
  { watch: [() => route.path] } // re-run when route changes
)

// Head should be safe even while loading
useHead(() => ({
  title: page.value?.title || 'Page',
  meta: [
    {
      name: 'description',
      content:
        page.value?.description ||
        page.value?.summary ||
        page.value?.excerpt ||
        ''
    }
  ]
}))

// Only throw a 404 after loading finishes
watchEffect(() => {
  if (!pending.value && !page.value) {
    throw createError({ statusCode: 404, statusMessage: 'Page not found' })
  }
})
</script>

<template>
  <section>
    <div class="container">
      <!-- Loading state -->
      <div v-if="pending" class="prose">
        <p class="muted">Loading…</p>
      </div>

      <!-- Render only when content exists -->
      <div v-else-if="page" class="prose">
        <h1>{{ page.title }}</h1>
        <ContentRenderer :value="page" />
      </div>
    </div>
  </section>
</template>
