<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences"
          >Load Submitted Experiences</base-button
        >
      </div>
      <p v-if="isLoading">Loading...</p>
      <p v-else-if="!isLoading && error">{{ error }}</p>
      <p v-else-if="!isLoading && (!results || results.length === 0)">No survey experiences found. Starting adding some survey results first.</p> 
      <ul v-else>
        <survey-result
          v-for="result in results"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
        ></survey-result>
      </ul>
    </base-card>
  </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue';
import axios from 'axios';

export default {
  components: {
    SurveyResult,
  },
  data() {
    return {
      results: [],
      isLoading: false,
      error: null
    };
  },
  methods: {
    loadExperiences() {
      // fetch('https://vue-http-demo-b99c4-default-rtdb.firebaseio.com/surveys.json')
      // this.isLoading = false;
      // .then((response) => {
      //   if(response.ok) {
      //     return response.json();
      //   }
      // })
      // .then((data) => {
      //   const results = [];
      //   for (const id in data) {
      //     results.push({
      //       id: id,
      //       name: data[id].name,
      //       rating: data[id].rating
      //     });
      //   }
      //   this.results = results;
      // });
      this.isLoading = true;
      this.error = null;
      axios.get('https://vue-http-demo-b99c4-default-rtdb.firebaseio.com/surveys.json')
          .then(({ data }) => {
            this.isLoading = false;
            this.results = Object.entries(data).map(([id, { name, rating }]) => ({ id, name, rating }));
        })
        .catch((error) => {
          console.log(error);
          this.isLoading = false;
          this.error = 'Unable to fetch data. Check the url for get request and try again.';
        });
    },
  },
  mounted() {
    this.loadExperiences();
  },
};

</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>
