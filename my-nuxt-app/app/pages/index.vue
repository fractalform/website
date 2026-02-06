<script setup>
useHead({
  title: 'Home',
  meta: [
    {
      name: 'description',
      content: 'Welcome to my fast, modern Nuxt website.'
    }
  ]
})
</script>
<template>
  <section class="hero">
    <div class="container">
    <h1>Welcome to My Nuxt Site</h1>
    <p>
      This site is built with Nuxt and loads instantly.
    </p>
    </div>
  </section>

  <section class="features">
    <div class="container">
  <FeatureCard
    title="Fast"
    description="Built with Vite and modern tooling."
  />
  <FeatureCard
    title="Simple"
    description="File-based routing and clean structure."
  />
  <FeatureCard
    title="Deployable"
    description="Works perfectly on static hosting."
  />
  <FeatureCard
    title="Scalable"
    description="Easy to grow from a small site to a full app."
  />
  </div>
</section>
</template>
<style scoped>
.hero {
  text-align: center;
}

.features .container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 2rem;
}
</style>