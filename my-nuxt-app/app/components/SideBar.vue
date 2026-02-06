<template>
  <aside class="sidebar">
    <div class="sidebar-inner">
      <div class="group">
        <div class="group-title">Navigation</div>
        <nav class="nav">
          <NuxtLink to="/" class="link">Home</NuxtLink>
          <NuxtLink to="/blog" class="link">Blog</NuxtLink>
          <NuxtLink to="/about" class="link">About</NuxtLink>
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
</style>