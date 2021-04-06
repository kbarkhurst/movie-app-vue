<template>
  <div class="movies-show">
    <div class="container py-3">
      <div class="row">
        <div v-if="movie.image" class="col-4">
          <img v-bind:src="movie.image" alt="movie.title" class="img-fluid" />
        </div>
        <div class="col">
          <h2>{{ movie.title }}</h2>
          <p>{{ movie.plot }}</p>
          <p>{{ movie.year }}</p>
          <hr />
          <div class="d-flex justify-content-end align-items-center" v-if="$parent.getUserId() == movie.user_id">
            <a class="text-decoration-none" v-on:click="destroyMovie(movie)">Delete Movie</a>
            <router-link v-bind:to="`/movies/${movie.id}/edit`" class="btn btn-success ms-3">
              <i class="bi bi-pencil-fill"></i>
              Edit this Movie
            </router-link>
          </div>
        </div>
      </div>
    </div>
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
