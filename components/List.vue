<template>
  <div id="list-container" class="list-group">
    <h2 id="top25Header">The Top 250</h2>
    <h3 id="underline">IMDB's list of the top 250 films in history</h3>
    <!--if faves, change line 7 to faves, line 9 etc-->
    <ul class="list-header">
      <li>About the list:</li>
      <li>- Click a movie to display more info</li>
      <li>- Click the star to favorite a movie</li>
      <form>
        <div id="radio-container">
          <div>
            <input
              type="radio"
              id="top250"
              value="top250"
              v-model="picked"
              @click="radioHandler()"
            />
            <label for="top250">Top 250</label>
          </div>
          <div>
            <input
              type="radio"
              id="faves"
              value="faves"
              v-model="picked"
              @click="radioHandler()"
            />
            <label for="faves">My Faves</label>
          </div>
        </div>
      </form>
    </ul>
    <div id="search-container">
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
      <li v-if="!isFiltered" v-for="(movie, index) in top250" :key="movie">
        <!--if found in favorites, switch to solid star-->
        <button @click="showMovie(movie)">{{ movie }}</button>
        <button class="favorite" @click="favorite(movie, index)">☆</button>
      </li>
      <!--add a way to search favorites-->
      <li v-if="isFiltered && picked == 'top250'" v-for="movie in filteredList">
        <button @click="showMovie(movie)">{{ movie }}</button>
        <button class="favorite" @click="favorite(movie)">☆</button>
      </li>
    </ul>
    <ul>
      <li v-if="faves" v-for="fave in faves" :key="fave">
        <button @click="showMovie(fave)">{{ fave }}</button>
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
      showFavorites: false,
      picked: "top250",
      favoriteIndex: null,
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
    favorite(movie, index) {
      // adds an element to the top of the list
      if (index > 0) {
        let element = this.top250[index];
        this.top250.splice(index, 1);
        this.top250.splice(0, 0, element);
      }
      this.faves.push(movie);
      console.log("Favorite button clicked!");
      console.log("favorited index is: " + index);
      // add control to prevent duplicate faves
    },
    clear() {
      this.isFiltered = false;
      this.searchClicked = false;
    },
    radioHandler() {
      console.log("hello from the radio handle!!!");
      if (this.picked == "top250") {
        this.picked = "faves";
        this.isFiltered = true;
      } else {
        this.picked = "top250";
        this.isFiltered = false;
      }
      console.log(this.picked);
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
  margin-top: 0.3rem;
  display: inline-block;
}
#radio-container {
  display: flex;
  flex-direction: row;
  padding: 0.2rem;
  justify-content: center;
  margin: 0.5rem 0 0.5rem 0;
}
#faves {
  margin-left: 15px;
}
#search-container {
  margin-bottom: 1rem;
}
#top25Header {
  margin: 1rem 0 1rem 0;
  font-size: 28px;
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