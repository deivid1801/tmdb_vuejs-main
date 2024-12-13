<script setup>
import { ref, onMounted } from 'vue'
import api from '@/plugins/axios'
import Loading from 'vue-loading-overlay'
import genreStore from '@/stores/genre'
import { useRouter } from 'vue-router'

const router = useRouter()
const isLoading = ref(false);
const tvs = ref([]);

const formatDate = (date) => new Date(date).toLocaleDateString('pt-BR')

const listTvs = async (genreId) => {
  genreStore.setCurrentGenreId(genreId);
  isLoading.value = true;
  const response = await api.get('discover/tv', {
    params: {
      with_genres: genreId,
      language: 'pt-BR'
    }
  });
  tvs.value = response.data.results
  isLoading.value = false;
};

function openTv(tvId) {
  router.push({ name: 'TvDetails', params: { tvId } });
}

onMounted(async () => {
  isLoading.value = true;
  await genreStore.getAllGenres('tv');
  isLoading.value = false;
})
</script>

<template>
  <div class="container">
    <h1 class="title">Programas de TV</h1>
    <ul class="genre-list">
      <li
        v-for="genre in genreStore.genres"
        :key="genre.id"
        @click="listTvs(genre.id)"
        class="genre-item"
        :class="{ active: genre.id === genreStore.currentGenreId }"
      >
        {{ genre.name }}
      </li>
    </ul>
    <loading v-model:active="isLoading" :can-cancel="false" :is-full-page="true" />
    <div class="tv-grid">
      <div v-for="tv in tvs" :key="tv.id" class="tv-card">
        <img :src="`https://image.tmdb.org/t/p/w500${tv.poster_path}`" :alt="tv.name" @click="openTv(tv.id)" />
        <div class="tv-details">
          <h2 class="tv-title">{{ tv.name }}</h2>
          <p class="tv-first-air-date">{{ formatDate(tv.first_air_date) }}</p>
          <div class="tv-genres">
            <span
              v-for="genre_id in tv.genre_ids"
              :key="genre_id"
              @click.stop="listTvs(genre_id)"
              :class="{ active: genre_id === genreStore.currentGenreId }"
            >
              {{ genreStore.getGenreName(genre_id) }}
            </span>
          </div>
        </div>
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

.title {
  font-size: 3rem;
  text-align: center;
  margin-bottom: 30px;
  color: var(--primary-color);
  text-shadow: 0 0 10px rgba(255, 215, 0, 0.5), 0 0 20px rgba(0, 119, 190, 0.3);
}

.genre-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 10px;
  margin-bottom: 30px;
}

.genre-item {
  background-color: var(--secondary-color);
  color: var(--text-color);
  padding: 8px 16px;
  border-radius: 20px;
  cursor: pointer;
  transition: all 0.3s ease;
  border: 2px solid var(--primary-color);
}

.genre-item:hover, .genre-item.active {
  background-color: var(--primary-color);
  color: var(--background-color);
  transform: scale(1.05);
  box-shadow: 0 0 15px rgba(255, 215, 0, 0.5);
}

.tv-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 20px;
}

.tv-card {
  background-color: var(--secondary-color);
  border-radius: 10px;
  overflow: hidden;
  transition: transform 0.3s ease;
  border: 2px solid var(--primary-color);
}

.tv-card:hover {
  transform: translateY(-5px) scale(1.03);
  box-shadow: 0 0 20px rgba(255, 215, 0, 0.4), 0 0 30px rgba(0, 119, 190, 0.3);
}

.tv-card img {
  width: 100%;
  height: 300px;
  object-fit: cover;
  cursor: pointer;
}

.tv-details {
  padding: 15px;
  background: linear-gradient(to bottom, var(--secondary-color), var(--background-color));
}

.tv-title {
  font-size: 1.2rem;
  margin-bottom: 5px;
  color: var(--primary-color);
  text-shadow: 0 0 5px rgba(255, 215, 0, 0.3);
}

.tv-first-air-date {
  font-size: 0.9rem;
  color: var(--accent-color);
  margin-bottom: 10px;
}

.tv-genres {
  display: flex;
  flex-wrap: wrap;
  gap: 5px;
}

.tv-genres span {
  background-color: var(--accent-color);
  color: var(--background-color);
  padding: 3px 8px;
  border-radius: 10px;
  font-size: 0.8rem;
  cursor: pointer;
  transition: all 0.3s ease;
}

.tv-genres span:hover, .tv-genres span.active {
  background-color: var(--primary-color);
  color: var(--background-color);
  box-shadow: 0 0 10px rgba(255, 215, 0, 0.5);
}

@media (max-width: 768px) {
  .tv-grid {
    grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
  }
}
</style>

