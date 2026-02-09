<template>
  <AppCard :to="props.to" class="preview" :class="{ 'no-thumb': !props.image }">

    <div v-if="image" class="thumb">
      <img :src="image" :alt="title" />
    </div>

    <div class="card-content">
      <div class="top">
        <h3 class="title">{{ title }}</h3>
        <p v-if="subtitle" class="subtitle">{{ subtitle }}</p>
      </div>

      <p class="excerpt">{{ excerpt }}</p>

      <div v-if="tags?.length" class="tags">
        <TagBadge v-for="t in tags" :key="t">{{ t }}</TagBadge>
      </div>
    </div>
  </AppCard>
</template>

<script setup lang="ts">
const props = defineProps<{
  to?: string
  title: string
  excerpt: string
  subtitle?: string
  image?: string
  tags?: string[]
}>()
</script>

<style scoped>
.preview {
  display: grid;
  grid-template-columns: 84px minmax(0, 1fr);
  gap: 1rem;
}

.preview {
  text-decoration: none;
}

/* When there is no image, don't reserve the 84px column */
.preview.no-thumb {
  grid-template-columns: 1fr;
}

.card-content {
  min-width: 0;
  width: 100%;
}

.preview:hover {
  transform: translateY(-2px);
  border-color: #d6d6d6;
  box-shadow: 0 10px 24px rgba(0, 0, 0, 0.08);
}

.preview:focus-visible {
  outline: 3px solid rgba(99, 102, 241, 0.35);
  outline-offset: 2px;
}

.thumb {
  width: 84px;
  height: 84px;
  border-radius: 12px;
  overflow: hidden;
  border: 1px solid var(--border);
  background: #f2f2f2;
}

.thumb img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.title,
.subtitle,
.excerpt {
  width: 100%;
}

.title {
  font-size: 1.05rem;
  margin: 0;
}

.subtitle {
  margin: 0.15rem 0 0 0;
  opacity: 0.75;
  font-size: 0.9rem;
}

/* excerpt should not be constrained like prose */
.excerpt {
  margin: 0.6rem 0 0.7rem 0;
  opacity: 0.9;
  max-width: none;
}

.tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.45rem;
}
</style>