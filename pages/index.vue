<template>
  <div>
    <header>
      <h1>M-Dash</h1>
      <p>The movie dashboard</p>
    </header>
    <div id="app-wrapper" class="center-block">
      <!--Make this into header component-->

      <div class="row">
        <List
          :top250="this.listData"
          @clickEvent="(msg) => (clickedMovie = msg)"
        />
        <MovieCard :movie="clickedMovie" />
      </div>
    </div>
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
    if (localStorage.length === 0) {
      console.log("no cached data found, calling IMDB API...");
      axios
        .get("https://imdb-api.com/en/API/Top250Movies/k_2uhdy54b")
        .then((response) => {
          this.rawData = JSON.stringify(response.data.items);
          this.handleData();
        });
    }
    this.exportToList();
    console.log("end of index mounted");
  },
  methods: {
    handleData() {
      //use a spread operater instead of for loop?
      console.log("handle data running...");
      let data = JSON.parse(this.rawData);
      for (let i = 0; i < data.length; i++) {
        localStorage.setItem(data[i].id, data[i].title);
      }
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
}
#app-wrapper {
  display: flex;
  flex-direction: row;
  justify-content: center;
  margin-left: 20vw;
}
header {
  padding-bottom: 10px;
  margin-bottom: 10px;
  text-align: center;
}
</style>
