<template>
  <div id="movie-container">
    <h1 id="cardHeader" class="cardItem">
      {{ this.retrievedTitle || "No movie selected" }}
    </h1>
    <img
      v-if="this.retrievedArt"
      class="cardItem"
      :src="`${this.retrievedArt}`"
      alt=""
    />
    <img v-else src="../assets/placeholder.png" />
    <h2 v-if="this.retrievedRating" class="cardItem">
      Rating: {{ this.retrievedRating }}
    </h2>
    <button v-if="this.movie" @click="getSum" class="summary-button">
      Click for summary
    </button>
    <div v-if="showSum" id="summary">
      <p>{{ this.retrievedSummary }}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  props: {
    movie: "",
  },
  data() {
    return {
      dataFromList: "",
      retrievedTitle: null,
      retrievedArt: null,
      retrievedRating: null,
      retrievedSummary: null,
      movieId: null,
      showSum: false,
    };
  },
  watch: {
    movie: function () {
      console.log(this.movie);
      this.fetchMovieDate();
    },
  },
  methods: {
    fetchMovieDate() {
      axios
        .get(`https://imdb-api.com/en/API/Search/k_2uhdy54b/${this.movie}`)
        .then((response) => {
          console.log(response);
          this.retrievedTitle = response.data.results[0].title;
          this.retrievedArt = response.data.results[0].image;
          console.log("fetching id///");
          console.log(this.retrievedTitle);
          console.log(localStorage.getItem(localStorage.key(3)));
          for (let i = 0; i < localStorage.length; i++) {
            if (
              localStorage.getItem(localStorage.key(i)) == this.retrievedTitle
            ) {
              console.log("movie found at: " + i);
              this.movieId = localStorage.key(i);
            }
          }
          axios
            .get(
              `https://imdb-api.com/en/API/Ratings/k_2uhdy54b/${this.movieId}`
            )
            .then((response) => {
              console.log(response);
              this.retrievedRating = response.data.imDb;
            });
        });
    },
    getSum() {
      this.showSum = true;
      axios
        .get(
          ` https://imdb-api.com/en/API/Wikipedia/k_2uhdy54b/${this.movieId}`
        )
        .then((response) => {
          console.log(response);
          this.retrievedSummary = response.data.plotShort.plainText;
        });
    },
  },
};
</script>

<style>
#movie-container {
  border: solid;
  border-width: 7px;
  border-radius: 5px;
  padding: 10px;
  max-width: 350px;
  max-height: 900px;
}
#cardHeader {
  margin: 0.5rem 0 1rem 0;
}
#summary {
  overflow-y: scroll;
  max-height: 200px;
}
img {
  height: 400px;
  width: 300px;
}
.cardItem {
  text-align: center;
  padding: 0.2rem;
}
.summary-button {
  border: solid;
  border-radius: 5px;
  padding: 3px;
  margin-bottom: 1rem;
}
</style>