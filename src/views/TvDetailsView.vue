<script setup>
import { onMounted } from 'vue';
import { useTvStore } from '@/stores/tv';

const tvStore = useTvStore();

const props = defineProps({
    tvId: {
        type: String,
        required: true,
    },
});

onMounted(async () => {
    await tvStore.getTvDetail(props.tvId);
});
</script>

<template>
    <div class="main">
        <div class="content">
            <img :src="`https://image.tmdb.org/t/p/w185${tvStore.currentTv.poster_path}`"
                :alt="tvStore.currentTv.name" />

            <div class="details">
                <h1>{{ tvStore.currentTv.name }}</h1>
                <p>Tagline: {{ tvStore.currentTv.tagline }}</p>
                <p>Overview {{ tvStore.currentTv.overview }}</p>
                <p>Avaliação: {{ tvStore.currentTv.vote_average }}</p>
            </div>
        </div>
    </div>

    <p>Produtoras</p>
    <div class="companies">
        <template v-for="company in tvStore.currentTv.production_companies" :key="company.id">
            <img v-if="company.logo_path" :src="`https://image.tmdb.org/t/p/w92${company.logo_path}`" :alt="company.name" />
            <p v-else>{{ company.name }}</p>
        </template>
    </div>
</template>

<style scoped>
.container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 2rem;
    background-color: #1b1b1b;
    color: #ffffff;
}

.content {
    display: flex;
    gap: 2rem;
    margin-bottom: 2rem;
}

.content img {
    width: 300px;
    height: auto;
    border-radius: 10px;
    box-shadow: 0 0 20px rgba(255, 153, 0, 0.3);
}

.details {
    flex: 1;
}

.title {
    font-size: 2.5rem;
    color: #ff9900;
    margin-bottom: 1rem;
}

.tagline {
    font-style: italic;
    color: #cccccc;
    margin-bottom: 1rem;
}

.overview {
    margin-bottom: 1rem;
    line-height: 1.6;
}

.info {
    font-size: 1.1rem;
    margin-bottom: 0.5rem;
}

.rating {
    background-color: #ff9900;
    color: #000000;
    padding: 0.2rem 0.5rem;
    border-radius: 5px;
    font-weight: bold;
}

.subtitle {
    font-size: 1.8rem;
    color: #ff9900;
    margin-bottom: 1rem;
}

.companies {
    display: flex;
    flex-wrap: wrap;
    gap: 2rem;
}

.company {
    background-color: #2d2d2d;
    padding: 1rem;
    border-radius: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 150px;
    height: 100px;
}

.company img {
    max-width: 100%;
    max-height: 100%;
    object-fit: contain;
}

@media (max-width: 768px) {
    .content {
        flex-direction: column;
    }

    .content img {
        width: 100%;
        max-width: 300px;
        margin: 0 auto;
    }
}
</style>