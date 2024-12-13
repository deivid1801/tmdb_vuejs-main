<script setup>
import { onMounted } from 'vue';
import { useMovieStore } from '@/stores/movie';

const movieStore = useMovieStore();

const props = defineProps({
  movieId: {
    type: String,
    required: true,
  },
});

onMounted(async () => {
  await movieStore.getMovieDetail(props.movieId);
});

const formatCurrency = (value) => {
  return new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD' }).format(value);
}
</script>

<template>
  <div class="container">
    <div class="movie-details">
      <img :src="`https://image.tmdb.org/t/p/w500${movieStore.currentMovie.poster_path}`" :alt="movieStore.currentMovie.title" class="movie-poster" />
      <div class="movie-info">
        <h1 class="movie-title">{{ movieStore.currentMovie.title }}</h1>
        <p class="movie-tagline">{{ movieStore.currentMovie.tagline }}</p>
        <p class="movie-overview">{{ movieStore.currentMovie.overview }}</p>
        <div class="movie-meta">
          <p><strong>Orçamento:</strong> {{ formatCurrency(movieStore.currentMovie.budget) }}</p>
          <p><strong>Avaliação:</strong> {{ movieStore.currentMovie?.vote_average?.toFixed(1) || 0 }} / 10</p>
        </div>
      </div>
    </div>
    <h2 class="section-title">Produtoras</h2>
    <div class="companies">
      <div v-for="company in movieStore.currentMovie.production_companies" :key="company.id" class="company-card">
        <img v-if="company.logo_path" :src="`https://image.tmdb.org/t/p/w92${company.logo_path}`" :alt="company.name" />
        <p v-else>{{ company.name }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
:root {
  --primary-color: #ffd700;
  --secondary-color: #0077be;
  --text-color: #ffffff;
  --background-color: #000000;
  --accent-color: #00aaff;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  background-color: var(--background-color);
  color: var(--text-color);
}

.movie-details {
  display: flex;
  gap: 30px;
  margin-bottom: 40px;
  background: linear-gradient(135deg, var(--secondary-color) 0%, var(--background-color) 100%);
  border-radius: 15px;
  padding: 30px;
  box-shadow: 0 0 30px rgba(0, 119, 190, 0.3);
}

.movie-poster {
  width: 300px;
  height: auto;
  border-radius: 10px;
  box-shadow: 0 0 20px rgba(255, 215, 0, 0.5);
  border: 3px solid var(--primary-color);
}

.movie-info {
  flex: 1;
}

.movie-title {
  font-size: 2.5rem;
  color: var(--primary-color);
  margin-bottom: 10px;
  text-shadow: 0 0 10px rgba(255, 215, 0, 0.5), 0 0 20px rgba(0, 119, 190, 0.3);
}

.movie-tagline {
  font-size: 1.2rem;
  font-style: italic;
  color: var(--accent-color);
  margin-bottom: 20px;
}

.movie-overview {
  font-size: 1rem;
  line-height: 1.6;
  margin-bottom: 20px;
  color: var(--text-color);
  text-shadow: 0 0 5px rgba(0, 0, 0, 0.5);
}

.movie-meta {
  display: flex;
  justify-content: space-between;
  font-size: 1rem;
  background-color: rgba(0, 119, 190, 0.3);
  padding: 15px;
  border-radius: 10px;
  border: 2px solid var(--primary-color);
}

.section-title {
  font-size: 2rem;
  color: var(--primary-color);
  margin-bottom: 20px;
  text-shadow: 0 0 10px rgba(255, 215, 0, 0.5), 0 0 20px rgba(0, 119, 190, 0.3);
}

.companies {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
}

.company-card {
  background-color: var(--secondary-color);
  padding: 15px;
  border-radius: 10px;
  text-align: center;
  width: 150px;
  border: 2px solid var(--primary-color);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.company-card:hover {
  transform: translateY(-5px) scale(1.05);
  box-shadow: 0 0 20px rgba(255, 215, 0, 0.4), 0 0 30px rgba(0, 119, 190, 0.3);
}

.company-card img {
  max-width: 100%;
  height: auto;
  margin-bottom: 10px;
}

@media (max-width: 768px) {
  .movie-details {
    flex-direction: column;
  }

  .movie-poster {
    width: 100%;
    max-width: 300px;
    margin: 0 auto;
  }
}
</style>

