<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import { useRoute } from 'vue-router';

const route = useRoute();
const currentRoute = ref(route.path);
const isScrolled = ref(false);

const updateCurrentRoute = () => {
  currentRoute.value = route.path;
};

const checkScroll = () => {
  isScrolled.value = window.scrollY > 50;
};

onMounted(() => {
  window.addEventListener('scroll', checkScroll);
});

onUnmounted(() => {
  window.removeEventListener('scroll', checkScroll);
});
</script>

<template>
  <header :class="{ 'scrolled': isScrolled }">
    <div class="logo">CineVerse</div>
    <nav>
      <router-link to="/" :class="{ 'active': currentRoute === '/' }">
        <span class="icon">🏠</span>
        <span class="text">Home</span>
      </router-link>
      <router-link to="/filmes" :class="{ 'active': currentRoute === '/filmes' }">
        <span class="icon">🎬</span>
        <span class="text">Filmes</span>
      </router-link>
      <router-link to="/tv" :class="{ 'active': currentRoute === '/tv' }">
        <span class="icon">📺</span>
        <span class="text">Series de TV</span>
      </router-link>
    </nav>
  </header>
  <main>
    <router-view @update:route="updateCurrentRoute" />
  </main>
</template>

<style scoped>
.text{
  color: #ffffff;
}
:root {
  --primary-color: #ffd700;
  --secondary-color: #0077be;
  --background-color: #000000;
  --text-color: #ffffff;
}

body {
  margin: 0;
  font-family: 'Arial', sans-serif;
  background-color: var(--background-color);
  color: var(--text-color);
}

header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  height: 4rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: rgba(0, 0, 0, 0.8);
  backdrop-filter: blur(10px);
  padding: 0 2rem;
  transition: all 0.3s ease;
  z-index: 1000;
}

header.scrolled {
  height: 3rem;
  background-color: rgba(0, 0, 0, 0.95);
  box-shadow: 0 2px 10px rgba(255, 215, 0, 0.2);
}

.logo {
  font-size: 1.5rem;
  font-weight: bold;
  color: var(--primary-color);
  text-shadow: 0 0 10px rgba(255, 215, 0, 0.5);
}

nav {
  display: flex;
  gap: 1.5rem;
}

nav a {
  text-decoration: none;
  color: var(--text-color);
  font-size: 1rem;
  padding: 0.5rem 1rem;
  border-radius: 20px;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

nav a:hover, nav a.active {
  background-color: var(--secondary-color);
  color: var(--primary-color);
  transform: translateY(-2px);
  box-shadow: 0 4px 15px rgba(0, 119, 190, 0.3);
}

nav a .icon {
  font-size: 1.2rem;
}

nav a .text {
  position: relative;
  overflow: hidden;
}

nav a .text::after {
  content: '';
  position: absolute;
  bottom: -2px;
  left: 0;
  width: 100%;
  height: 2px;
  background-color: var(--primary-color);
  transform: translateX(-100%);
  transition: transform 0.3s ease;
}

nav a:hover .text::after, nav a.active .text::after {
  transform: translateX(0);
}

main {
  margin-top: 4rem;
  padding: 2rem;
}

@media (max-width: 768px) {
  header {
    flex-direction: column;
    height: auto;
    padding: 1rem;
  }

  .logo {
    margin-bottom: 1rem;
  }

  nav {
    width: 100%;
    justify-content: space-around;
  }

  nav a {
    flex-direction: column;
    font-size: 0.8rem;
  }

  nav a .icon {
    font-size: 1.5rem;
  }

  main {
    margin-top: 7rem;
  }
}
</style>