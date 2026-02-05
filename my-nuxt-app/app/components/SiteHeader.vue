<template>
  <header>
    <h2>My Nuxt Website</h2>

    <nav>
      <NuxtLink to="/">Home</NuxtLink>
      <NuxtLink to="/about">About</NuxtLink>
    </nav>
  </header>
</template>

<style scoped>
header {
  background: #222;
  color: white;
  padding: 1rem;
}

nav a {
  margin-right: 1rem;
  color: white;
}
</style>