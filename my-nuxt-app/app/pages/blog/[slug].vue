<script setup lang="ts">
const route = useRoute()

const { data: post, pending } = useAsyncData(
  () => `blog:${route.path}`,
  () => queryCollection('blog').path(route.path).first(),
  { watch: [() => route.path] }
)

useHead(() => ({
  title: post.value?.title || 'Post',
  meta: [
    {
      name: 'description',
      content:
        post.value?.description ||
        post.value?.summary ||
        post.value?.excerpt ||
        ''
    }
  ]
}))

watchEffect(() => {
  if (!pending.value && !post.value) {
    throw createError({ statusCode: 404, statusMessage: 'Post not found' })
  }
})
</script>

<template>
  <section>
    <div class="container">
      <div v-if="pending" class="prose">
        <p class="muted">Loading…</p>
      </div>

      <div v-else-if="post" class="prose">
        <h1>{{ post.title }}</h1>
        <p v-if="post.date" class="muted"><em>{{ post.date }}</em></p>
        <ContentRenderer :value="post" />
      </div>
    </div>
  </section>
</template>
