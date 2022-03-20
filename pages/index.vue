<template>
  <div>
    <!--Make this into header component-->
    <h1>M-Dash</h1>
    <p>The movie dashboard</p>
    <List />
  </div>
</template>

<script>
// a scrolling poster of movies before user enters movie

// libraries etc.
import axios from "axios";
// components
import Movie from "../components/Movie.vue";
import List from "../components/List.vue";
export default {
  data() {
    return {};
  },
  mounted() {
    axios;

    localStorage.setItem("test", 1);
    console.log("index mounted");
    //.get("https://imdb-api.com/en/API/Top250Movies/k_2uhdy54b")
    //.then((response) => console.log(response));
  },
};
</script>



<style>
* {
  background-color: cyan;
  text-align: center;
}
</style>
