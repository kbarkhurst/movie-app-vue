<template>
  <div class="movies-show">
    <div class="container">
      <h2>{{ movie.title }}</h2>
      <p>{{ movie.plot }}</p>
      <p>{{ movie.year }}</p>
    </div>
    <router-link v-bind:to="`/movies/${movie.id}/edit`">Edit this Movie</router-link>
    <br />
    <button v-on:click="destroyMovie(movie)">Destroy Movie</button>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      movie: {},
    };
  },
  created: function () {
    this.showMovies();
  },
  methods: {
    showMovies: function () {
      axios.get("/api/movies/" + this.$route.params.id).then((response) => {
        console.log(response.data);
        this.movie = response.data;
      });
    },
    destroyMovie: function (movie) {
      axios.delete("/api/movies/" + movie.id).then(() => {
        console.log("Movie Deleted");
        this.$router.push("/movies");
      });
    },
  },
};
</script>
