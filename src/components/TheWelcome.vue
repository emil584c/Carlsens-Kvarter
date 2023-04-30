<script setup>
import { ref } from 'vue'
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
          heroDescription {value}
        }
      }
      aboutSection{
      ... on AboutRecord{
        sectionTitle
        aboutText {value}
        aboutImage {
          id
        }
      }
  	}
    }
  }
`;

request({ query: HOMEPAGE_QUERY }).then(result => {
  data.value = result;
  console.log(data.value.homePage.heroContent);
}).catch(e => {
  error.value = e;
}).finally(() => {
  loading.value = false;
});

</script>

<template>
  <div>
    <div v-if="loading">Loading...</div>
    <div v-else-if="error">Something bad happened</div>
    <div v-else>
      <h1> y{{ data.homePage.name }}</h1>
      <p>{{ data.homePage.heroContent[0].heroText }}</p>
    </div>
  </div>
</template>
