<template>
  <div>
    <form @submit="onSubmit">
      <input v-model="search_input" placeholder="Search movies" />
      <input type="submit" value="Search" />
    </form>
    <ul>
      <li v-for="movie in movies" :key="movie.title">
        {{ movie.title }}
        <button @click="favorite(movie)">Favorite</button>
      </li>
    </ul>
  </div>
</template>

<!--on search, first js function runs to check cache to see if movie title is found, if not, query the api-->

<script>
import axios from "axios";
export default {
  // api key: k_2uhdy54b
  data() {
    return {
      search_input: null,
      movies: [
        {
          title: "The Shawshank Redemption",
          title: "Titanictic",
          title: "Schindler's List",
        },
      ],
      favorites: [],
    };
  },
  mounted() {
    axios;
    //.get("https://imdb-api.com/en/API/Search/k_2uhdy54b/inception 2010")
    //.then((response) => console.log(response));
    console.log("List mounted");
    // use movie list to populate meta data for seo
  },
  methods: {
    onSubmit(e) {
      e.preventDefault();
      console.log(this.search_input);
      this.queryAPI();
    },
    queryAPI() {
      let search = this.search_input;
      console.log("hello from the query" + search);
    },
    favorite(movie) {
      this.favorites.push(movie);
      console.log("Favorites :" + toString(this.favorites));
    },
  },
};
</script>

<style>
input {
  background-color: white;
}
</style>