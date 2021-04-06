<template>
  <div class="movies-edit">
    <div class="container py-3">
      <form v-on:submit.prevent="updateMovie(movie)">
        <h1>Edit this Movie</h1>
        <ul>
          <li class="text-danger" v-for="error in errors" v-bind:key="error">
            {{ error }}
          </li>
        </ul>
        <div class="form-group">
          <label>Title:</label>
          <input type="text" class="form-control" v-model="movie.title" />
        </div>
        <div class="form-group">
          <label>Plot:</label>
          <input type="text" class="form-control" v-model="movie.plot" />
        </div>
        <div class="form-group">
          <label>Year:</label>
          <input type="text" class="form-control" v-model="movie.year" />
        </div>
        <div class="form-group">
          <label>Image:</label>
          <input type="text" class="form-control" v-model="movie.image" />
        </div>
        <input type="submit" class="btn btn-primary my-2" value="Submit" />
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      movie: {},
      errors: [],
    };
  },
  created: function () {
    axios.get("/api/movies/" + this.$route.params.id).then((response) => {
      console.log(response.data);
      this.movie = response.data;
    });
  },
  methods: {
    updateMovie: function (movie) {
      console.log("Updating this movie");
      var params = {
        title: movie.title,
        plot: movie.plot,
        year: movie.year,
        image: movie.image,
      };
      axios
        .patch("/api/movies/" + this.$route.params.id, params)
        .then(() => {
          this.$router.push("/movies/" + this.movie.id);
        })
        .catch((error) => console.log(error.response));
    },
  },
};
</script>
