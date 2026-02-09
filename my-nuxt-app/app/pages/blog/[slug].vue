<script setup lang="ts">
const route = useRoute()

const { data: post } = await useAsyncData(route.path, () =>
  queryCollection('blog').path(route.path).first()
)

if (!post.value) {
  throw createError({ statusCode: 404, statusMessage: 'Post not found' })
}

useHead({
  title: post.value.title || 'Post',
  meta: [{ name: 'description', content: post.value.description || post.value.excerpt || '' }]
})
</script>

<template>
  <section>
    <div class="container prose">
      <h1>{{ post.title }}</h1>
      <p v-if="post.date"><em>{{ post.date }}</em></p>
      <ContentRenderer :value="post" />
    </div>
  </section>
</template>
