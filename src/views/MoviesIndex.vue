<template>
  <div class="home">
    <div>
      <div v-for="movie in movies" v-bind:key="movie.id">
        <router-link title="More about this Movie" v-bind:to="`movies/${movie.id}`">
          <h3>{{ movie.title }}</h3>
          <p>{{ movie.plot }}</p>
          <p>{{ movie.year }}</p>
          <hr/>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      movies: [],
    };
  },
  created: function () {
    axios.get("/api/movies").then((response) => {
      this.movies = response.data;
      console.log("all movies:", this.movies);
    });
  },
  methods: {
    // indexMovies: function () {
    //   axios.get("/api/movies").then((response) => {
    //     this.movies = response.data;
    //     console.log("all movies:", this.movies);
    //   });
    // },
  },
};
</script>
