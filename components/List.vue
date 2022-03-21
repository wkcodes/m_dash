<template>
  <div>
    <form @submit="onSubmit">
      <input v-model="search_input" placeholder="Search movies" />
      <input type="submit" value="Search" />
    </form>
    <h2>The Top 250</h2>
    <!--if faves, change line 7 to faves, line 9 etc-->
    <p>IMDB's list of the top 250 films in history</p>
    <br />
    <ul>
      <li v-for="movie in top250" :key="movie">
        {{ movie }}
        <button class="favorite" @click="favorite(movie)">Favorite</button>
      </li>
    </ul>
    <h2>My Faves</h2>
    <ul>
      <li v-for="fave in faves" :key="fave">
        {{ fave }}
      </li>
    </ul>
  </div>
</template>

<!--on search, first js function runs to check cache to see if movie title is found, if not, query the api-->

<script>
import axios from "axios";
export default {
  props: {
    top250: Array,
  },
  data() {
    return {
      search_input: null,
      faves: [],
    };
  },

  mounted() {
    console.log("List mounted");
    // use movie list to populate meta data for seo
  },
  methods: {
    onSubmit(e) {
      e.preventDefault();
      console.log(this.search_input);
      this.search();
    },
    search() {
      //search the list
    },
    favorite(movie) {
      // adds an element to the top of the list
      this.top250.unshift(movie);
      this.faves.push(movie);
      console.log("Favorite button clicked!");
      // add control to prevent duplicate faves
    },
  },
  // for description, check if exists, if not call search api
};
</script>

<style>
input {
  border-style: dashed;
  border-color: tomato;
}
.favorite {
  color: orange;
}
</style>