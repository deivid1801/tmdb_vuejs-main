<script setup>
import { ref, onMounted } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();

// Função para gerar o caminho da imagem com base no número
const getImagePath = (index) => {
  return `/images/image${index}.jpg`; // Supondo que as imagens estejam em /images/
};

const featuredMovies = ref([
  { id: 1, title: 'Inception', image: getImagePath(1), rating: 8.8 },
  { id: 2, title: 'The Shawshank Redemption', image: getImagePath(2), rating: 9.3 },
  { id: 3, title: 'The Dark Knight', image: getImagePath(3), rating: 9.0 },
]);

const featuredTVSeries = ref([
  { id: 1, title: 'Breaking Bad', image: getImagePath(4), rating: 9.5 },
  { id: 2, title: 'Game of Thrones', image: getImagePath(5), rating: 9.2 },
  { id: 3, title: 'Stranger Things', image: getImagePath(6), rating: 8.7 },
]);

const navigateToMovies = () => {
  router.push('/filmes');
};

const navigateToTVSeries = () => {
  router.push('/tv');
};

onMounted(() => {
  const observer = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        entry.target.classList.add('show');
      }
    });
  }, { threshold: 0.1 });

  document.querySelectorAll('.fade-in').forEach((el) => observer.observe(el));
});
</script>

<template>
  <div class="home-container">
    <section class="hero">
      <div class="hero-content">
        <h1 class="hero-title fade-in">Bem-vindo ao CineVerse</h1>
        <p class="hero-subtitle fade-in" style="color: aliceblue;">Descubra os melhores filmes e séries de TV</p>
      </div>
    </section>

    <section class="featured-section fade-in">
      <h2 class="section-title">Filmes em Destaque</h2>
      <div class="featured-grid">
        <div v-for="movie in featuredMovies" :key="movie.id" class="featured-item">
          <img :src="movie.image" :alt="movie.title" class="featured-image" />
          <div class="featured-overlay" :style="{ backgroundImage: 'url(' + movie.image + ')' }">
            <h3 class="featured-title">{{ movie.title }}</h3>
            <span class="featured-rating">⭐ {{ movie.rating }}</span>
          </div>
        </div>
      </div>
      <button @click="navigateToMovies" class="view-more-btn">Ver Mais Filmes</button>
    </section>

    <section class="featured-section fade-in">
      <h2 class="section-title">Séries de TV em Destaque</h2>
      <div class="featured-grid">
        <div v-for="series in featuredTVSeries" :key="series.id" class="featured-item">
          <img :src="series.image" :alt="series.title" class="featured-image" />
          <div class="featured-overlay" :style="{ backgroundImage: 'url(' + series.image + ')' }">
            <h3 class="featured-title">{{ series.title }}</h3>
            <span class="featured-rating">⭐ {{ series.rating }}</span>
          </div>
        </div>
      </div>
      <button @click="navigateToTVSeries" class="view-more-btn">Ver Mais Séries</button>
    </section>
  </div>
</template>

<style scoped>
:root {
  --primary-color: #ffd700;
  --secondary-color: #0077be;
  --background-color: #121212;
  --text-color: #ffffff;
  --accent-color: #00aaff;
}

.home-container {
  background-color: var(--background-color);
  color: var(--text-color);
  min-height: 100vh;
  font-family: 'Arial', sans-serif;
  margin: 0;
}

.hero {
  height: 80vh;
  background-image: url('/Cine_Verce.jpeg');
  background-size: cover;
  background-position: center;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  box-shadow: inset 0 0 50px rgba(0, 0, 0, 0.7);
}

.hero::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(to bottom, rgba(0, 0, 0, 0.6), rgba(0, 0, 0, 0.2));
}

.hero-content {
  text-align: center;
  z-index: 1;
}

.hero-title {
  font-size: 4rem;
  font-weight: bold;
  margin-bottom: 1rem;
  color: var(--primary-color);
  text-shadow: 0 0 20px rgba(255, 215, 0, 0.7), 0 0 30px rgba(0, 119, 190, 0.4);
  animation: fadeIn 2s ease-in-out;
}

.hero-subtitle {
  font-size: 1.5rem;
  color: var(--text-color);
  text-shadow: 0 0 15px rgba(0, 0, 0, 0.6);
  animation: fadeIn 2s ease-in-out 0.5s;
}

@keyframes fadeIn {
  0% {
    opacity: 0;
    transform: translateY(20px);
  }
  100% {
    opacity: 1;
    transform: translateY(0);
  }
}

.featured-section {
  padding: 5rem 2rem;
  animation: fadeIn 1s ease-in-out;
}

.section-title {
  font-size: 2.8rem;
  color: var(--primary-color);
  text-align: center;
  margin-bottom: 3rem;
  text-shadow: 0 0 15px rgba(255, 215, 0, 0.4);
}

.featured-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 2rem;
  margin-bottom: 2rem;
}

.featured-item {
  position: relative;
  overflow: hidden;
  border-radius: 12px;
  box-shadow: 0 0 25px rgba(0, 119, 190, 0.3);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.featured-item:hover {
  transform: scale(1.05);
  box-shadow: 0 0 35px rgba(255, 215, 0, 0.5);
}

.featured-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.featured-overlay {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  background: linear-gradient(to top, rgba(0, 0, 0, 0.9), rgba(0, 0, 0, 0));
  padding: 1rem;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.featured-item:hover .featured-overlay {
  opacity: 1;
}

.featured-title {
  font-size: 1.3rem;
  color: var(--primary-color);
  margin-bottom: 0.5rem;
  font-weight: bold;
  transition: color 0.3s ease;
}

.featured-rating {
  font-size: 1.1rem;
  color: var(--text-color);
}

.view-more-btn {
  display: block;
  margin: 0 auto;
  padding: 1rem 2.5rem;
  font-size: 1.2rem;
  background-color: var(--secondary-color);
  color: var(--text-color);
  border: none;
  border-radius: 50px;
  cursor: pointer;
  transition: all 0.
}</style>