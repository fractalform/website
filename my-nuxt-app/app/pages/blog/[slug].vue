<script setup lang="ts">
import { posts } from '~/data/posts'

const route = useRoute()
const post = posts.find(p => p.slug === route.params.slug)

if (!post) {
  throw createError({ statusCode: 404, statusMessage: 'Post not found' })
}

useHead({
  title: post.title,
  meta: [{ name: 'description', content: post.excerpt }]
})
</script>

<template>
  <section>
    <div class="container prose">
      <h1>{{ post.title }}</h1>
      <p><em>{{ post.date }}</em></p>
      <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
    </div>
  </section>
</template>