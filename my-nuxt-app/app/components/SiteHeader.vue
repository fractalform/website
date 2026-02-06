<template>
  <header class="site-header">
    <div class="container">
      <h2>My Nuxt Website</h2>
      <nav>
        <NuxtLink to="/">Home</NuxtLink>
        <NuxtLink to="/about">About</NuxtLink>
      </nav>
    </div>
  </header>
</template>

<style scoped>
.site-header {
  background: #222;
  color: white;
  padding: 1rem 0; /* vertical padding lives here */
}

.site-header .container {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

nav a {
  margin-left: 1rem;
  color: white;
  text-decoration: none;
}

nav a.router-link-active {
  text-decoration: underline;
}
</style>