<script setup lang="ts">
import { navItems } from '~/data/nav'
import { posts } from '~/data/posts'

const route = useRoute()
const router = useRouter()

const tags = Array.from(new Set(posts.flatMap(p => p.tags))).sort()

function toggleTag(tag: string) {
  router.push({
    query: tag === route.query.tag ? {} : { tag }
  })
}
</script>

<template>
  <aside class="sidebar">
    <div class="sidebar-inner">
      <div class="group">
        <div class="group-title">Navigation</div>
            <nav class="nav">
            <NuxtLink v-for="i in navItems" :key="i.to" :to="i.to" class="link">
                {{ i.label }}
            </NuxtLink>
            </nav>
      </div>

      <div class="group">
        <div class="group-title">Categories</div>
        <button class="pill">UI</button>
        <button class="pill">Guides</button>
        <button class="pill">Updates</button>
      </div>

      <div class="group">
        <div class="group-title">Tags</div>
        <div class="pill-row">
          <button class="pill">nuxt</button>
          <button class="pill">design</button>
          <button class="pill">hosting</button>
        </div>
      </div>

      <div class="filter-section">
        <h4>Filter</h4>

        <button
            v-for="tag in tags"
            :key="tag"
            class="tag-filter"
            :class="{ active: route.query.tag === tag }"
            @click="toggleTag(tag)"
        >
            {{ tag }}
        </button>
        </div>
    </div>
  </aside>
</template>

<style scoped>
.sidebar-inner {
  padding: 1.25rem 1rem;
}

.group + .group {
  margin-top: 1.25rem;
}

.group-title {
  font-size: 0.9rem;
  font-weight: 600;
  opacity: 0.75;
  margin-bottom: 0.75rem;
}

.nav {
  display: grid;
  gap: 0.4rem;
}

.link {
  padding: 0.55rem 0.65rem;
  border-radius: 8px;
  text-decoration: none;
  color: inherit;
}

.link:hover {
  background: #f3f4f6;
}

.link.router-link-active {
  background: #eef2ff;
}

.pill-row {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}

.pill {
  border: 1px solid #e7e7e7;
  background: #fafafa;
  padding: 0.4rem 0.6rem;
  border-radius: 999px;
  font-size: 0.85rem;
  cursor: pointer;
}

.pill:hover {
  background: #f3f4f6;
}

.filter-section {
  margin-top: 1.5rem;
}

.filter-section h4 {
  font-size: 0.85rem;
  text-transform: uppercase;
  opacity: 0.6;
  margin-bottom: 0.5rem;
}

.tag-filter {
  display: block;
  width: 100%;
  text-align: left;
  padding: 0.4rem 0.6rem;
  border-radius: 8px;
  border: none;
  background: transparent;
  cursor: pointer;
}

.tag-filter:hover {
  background: rgba(0,0,0,0.05);
}

.tag-filter.active {
  background: rgba(0,0,0,0.1);
  font-weight: 600;
}
</style>