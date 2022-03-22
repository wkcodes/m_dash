<template>
  <div id="movie-container">
    <h1 class="cardItem">{{ this.retrievedTitle || "Click a movie" }}</h1>
    <img class="cardItem" :src="`${this.retrievedArt}`" alt="" />
    <h2 class="cardItem">Rating: {{ this.retrievedRating }}</h2>
    <button class="summary-button cardItem">Click for summary</button>
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
img {
  height: 400px;
  width: 300px;
}
.cardItem {
  padding: 0.2rem;
}
.summary-button {
  border: solid;
  border-radius: 5px;
  padding: 3px;
}
</style>