<template>
  <section class="hero">
    <h1>Welcome to My Nuxt Site</h1>
    <p>
      This site is built with Nuxt and loads instantly.
    </p>
  </section>

  <section class="features">
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
</section>
</template>
<style scoped>
.hero {
  padding: 4rem 2rem;
  text-align: center;
}

.features {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 2rem;
  padding: 2rem;
}

.feature {
  padding: 1.5rem;
  border: 1px solid #ddd;
  border-radius: 8px;
}
</style>