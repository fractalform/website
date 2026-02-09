<script setup lang="ts">
const route = useRoute()

const { data: page } = await useAsyncData(route.path, () => {
  return queryCollection('pages').path(route.path).first()
})

if (!page.value) {
  throw createError({ statusCode: 404, statusMessage: 'Page not found' })
}

useHead({
  title: page.value.title,
  meta: [{ name: 'description', content: page.value.description || page.value.excerpt || '' }]
})
</script>

<template>
  <section>
    <div class="container prose">
      <h1>{{ page.title }}</h1>
      <ContentRenderer :value="page" />
    </div>
  </section>
</template>
