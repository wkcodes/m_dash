<template>
  <div>
    <form @submit="onSubmit">
      <input v-model="search_input" placeholder="Search movies" />
      <input type="submit" value="Search" />
    </form>
  </div>
</template>

<script>
import axios from "axios";
export default {
  // api key: k_2uhdy54b
  data() {
    return {
      search_input: null,
      movies: ["the matrix", "flintstones", "home alone"],
    };
  },
  mounted() {
    axios;
    //.get("https://imdb-api.com/en/API/Search/k_2uhdy54b/inception 2010")
    //.then((response) => console.log(response));
    console.log("Search components mounted");
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
  },
};
</script>

<style>
input {
  background-color: white;
}
</style>