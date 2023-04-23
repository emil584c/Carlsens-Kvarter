<script setup>
import { ref, onMounted } from 'vue'
import { request } from "../datocms";

const data = ref(null);
const error = ref(null);
const loading = ref(true);

const HOMEPAGE_QUERY = `
  query HomePage {
    homePage {
      id
      name
      heroContent {
        ... on HeroRecord {
          heroText
        }
      }
    }
  }
`;

onMounted(async () => {
  try {
    data.value = await request({ query: HOMEPAGE_QUERY });
    console.log(data.value.homePage.heroContent);
  } catch (e) {
    error.value = e;
  }
  loading.value = false;
});
</script>

<template>
  <div>
    <div v-if="loading">Loading...</div>
    <div v-else-if="error">Something bad happened</div>
    <div v-else>
      <h1>{{ data.homePage.name }}</h1>
      <p>{{ data.homePage.heroContent.heroText }}</p>
    </div>
  </div>
</template>
