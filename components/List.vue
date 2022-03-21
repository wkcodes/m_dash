<template>
  <div id="list-container" class="list-group">
    <h2>The Top 250</h2>
    <h3>IMDB's list of the top 250 films in history</h3>
    <!--if faves, change line 7 to faves, line 9 etc-->
    <ul class="list-header">
      <li>About the list:</li>
      <li>- Click a movie to display more info</li>
      <li>- Click the star to favorite a movie</li>
      <li>- Toggle top 250/faves</li>
    </ul>
    <form @submit="onSubmitSearch">
      <input
        class="search-controls"
        v-model="searchInput"
        placeholder="Enter movie title"
      />
      <input class="search-controls" type="submit" value="Search" />
      <button class="search-controls">Clear</button>
    </form>
    <ul>
      <li v-if="!isFiltered" v-for="movie in top250" :key="movie">
        <button @click="showMovie(movie)">{{ movie }}</button>
        <button class="favorite" @click="favorite(movie)">⭐</button>
      </li>
      <li v-if="isFiltered" v-for="movie in filteredList">
        <button @click="showMovie(movie)">{{ movie }}</button>
        <button class="favorite" @click="favorite(movie)">⭐</button>
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

<!--on search, first js function runs to check cache to see if movie title is found, if not, query the api, test-->

<script>
import axios from "axios";
export default {
  props: {
    top250: Array,
  },
  data() {
    return {
      searchInput: null,
      filteredList: null,
      isFiltered: false,
      searchResult: null,
      faves: [],
    };
  },
  mounted() {
    console.log("List mounted");
    // use movie list to populate meta data for seo
  },
  methods: {
    onSubmitSearch(e) {
      e.preventDefault();
      console.log(this.searchInput);
      this.filteredList = this.top250.filter(
        (item) => item == this.searchInput
      );
      console.log(this.filteredList);
      this.isFiltered = true;
    },
    showMovie() {
      console.log("show movie clicked");
      axios
        .get(
          `https://imdb-api.com/en/API/Search/k_2uhdy54b/${this.searchInput}`
        )
        .then((response) => {
          console.log(response);
          this.showMovieHandler;
        });
    },
    showMovieHandler() {
      console.log("hello from the handler");
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
#list-container {
  overflow-y: scroll;
  max-height: 80vh;
  max-width: 50vw;
  border-style: solid;
  border-width: 7px;
  border-radius: 10px;
  padding: 10px;
  margin-left: 5vw;
  margin-right: 10px;
}
#search {
  padding: 5px;
}
.list-header {
  padding: 5px;
}
.search-controls {
  border-style: solid;
  border-color: black;
  border-radius: 5px;
  padding: 3px;
}
.favorite {
  color: orange;
}
</style>