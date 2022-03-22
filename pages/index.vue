<template>
  <div>
    <header>
      <h1 id="m-dash">M-Dash</h1>
      <h2>The movie dashboard</h2>
    </header>
    <div id="app-wrapper">
      <div id="list">
        <List
          :top250="this.listData"
          @clickEvent="(msg) => (clickedMovie = msg)"
        />
      </div>
      <div id="movieCard">
        <MovieCard :movie="clickedMovie" />
      </div>
    </div>
    <footer>
      <p class="imdbLink">
        See more at <a href="https://www.imdb.com/">IMDB</a>
      </p>
      <p>Made with ♡ by WKCODES 2022</p>
    </footer>
  </div>
</template>

<script>
/*

---IMPORTANT, make a toggle for my faves and top 250 

*/
// libraries etc.
import axios from "axios";
import BootstrapVue from "bootstrap-vue/dist/bootstrap-vue.esm";
// components
import MovieCard from "../components/MovieCard.vue";
import List from "../components/List.vue";
export default {
  components: {
    List,
    MovieCard,
  },
  data() {
    return {
      rawData: [],
      listData: [],
      clickedMovie: null,
    };
  },
  mounted() {
    console.log("start of index mounted");
    //check if loacal storage is empty
    console.log("calling IMDB API...");
    axios
      .get("https://imdb-api.com/en/API/Top250Movies/k_2uhdy54b")
      .then((response) => {
        this.rawData = JSON.stringify(response.data.items);
        this.handleData();
      });
    console.log("end of index mounted");
  },
  updated() {
    if (this.listData.length === 0) {
      console.log("empty from watcher");
      this.$forceUpdate();
    }
  },
  methods: {
    handleData() {
      //use a spread operater instead of for loop?
      console.log("handle data running...");
      let data = JSON.parse(this.rawData);
      for (let i = 0; i < data.length; i++) {
        localStorage.setItem(data[i].id, data[i].title);
      }
      this.exportToList();
    },
    exportToList() {
      // loop through local storage to get movies
      let keys = Object.keys(localStorage);
      for (let i = 0; i < keys.length; i++) {
        this.listData.push(localStorage.getItem(keys[i]));
      }
    },
  },
};
</script>



<style>
* {
  list-style-type: none;
  text-decoration: none;
}
#app-wrapper {
  display: grid;
  grid-template-columns: 50% 50%;
  grid-template-rows: 50% 50%;
  gap: 5rem;
  height: 85vh;
}
header {
  padding-bottom: 10px;
  margin-top: 1rem;
  margin-bottom: 10px;
  text-align: center;
}
footer {
  display: absolute;
  bottom: 0;
  text-align: center;
  margin-top: 10px;
  margin-bottom: 1rem;
}
.imdbLink {
  text-align: center;
}
a {
  color: goldenrod;
}
#m-dash {
  color: goldenrod;
  font-size: 3rem;
}
#list {
  grid-column-start: 1;
  grid-column-end: 1;
  justify-self: end;
}
#movieCard {
  grid-column-start: 2;
  grid-column-end: 2;
  justify-self: start;
}
</style>
