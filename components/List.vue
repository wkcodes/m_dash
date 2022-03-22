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
    <div>
      <form @submit="onSubmitSearch">
        <input
          class="search-controls"
          v-model="searchInput"
          placeholder="Enter movie title"
        />
        <input class="search-controls" type="submit" value="Search" />
      </form>
      <button
        v-if="searchClicked"
        @click="clear()"
        id="clear-button"
        class="search-controls"
      >
        Clear Search
      </button>
    </div>
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
  emits: ["clickEvent"],
  data() {
    return {
      searchInput: null,
      filteredList: null,
      isFiltered: false,
      searchResult: null,
      clickedMovie: null,
      faves: [],
      searchClicked: false,
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
      this.searchClicked = true;
    },
    showMovie(movie) {
      console.log("show movie clicked");
      this.clickedMovie = movie;
      console.log(this.clickedMovie);
      this.$emit("clickEvent", this.clickedMovie);
    },
    favorite(movie) {
      // adds an element to the top of the list
      this.top250.unshift(movie);
      this.faves.push(movie);
      console.log("Favorite button clicked!");
      // add control to prevent duplicate faves
    },
    clear() {
      this.isFiltered = false;
      this.searchClicked = false;
    },
  },
  // for description, check if exists, if not call search api
};
</script>

<style>
#list-container {
  overflow-y: scroll;
  white-space: nowrap;
  max-height: 80vh;
  max-width: 40vw;
  border-style: solid;
  border-width: 7px;
  border-radius: 10px;
  padding: 10px;
  text-align: center;
}
#search {
  padding: 5px;
}
#clear-button {
  display: inline-block;
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