<template>
  <div>
    <!--Make this into header component-->
    <h1>M-Dash</h1>
    <p>The movie dashboard</p>
    <ul>
      <li v-for="movie in movies">
        {{ movie }}
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";
export default {
  // api key: k_2uhdy54b
  data() {
    return {
      info: null,
      movies: ["the matrix", "flintstones", "home alone"],
    };
  },
  mounted() {
    axios;
    //.get("https://imdb-api.com/en/API/Search/k_2uhdy54b/inception 2010")
    //.then((response) => console.log(response));
  },
};
</script>

<style>
</style>
