<template>
  <div class="home">
    <div class="bg-light p-5 m-3">
      <div class="row">
        <div class="col-md-4 d-flex align-items-center">
          <div class="ratio ratio-16x9">
            <iframe
              width="560"
              height="315"
              src="https://www.youtube.com/embed/qM79_itR0Nc"
              title="YouTube video player"
              frameborder="0"
              allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
              allowfullscreen
            ></iframe>
          </div>
        </div>
        <div class="col-md-8">
          <h1 class="display-4">Welcome to Movie Phone</h1>
          <p class="lead">This is a simple-to-use web app for adding and cataloging your favorite movies.</p>
          <hr class="my-4" />
          <!-- <p>You can add, edit and explore movies.</p> -->
          <a class="btn btn-primary btn-lg" href="/movies" role="button">Check It Out!</a>
        </div>
      </div>
    </div>

    <!-- <h1>{{ message }}</h1>
    <ul
      v-if="errors.length"
      style="
        color: red;
        list-style: none;
        text-align: center;
        border: solid 1px red;
        width: 70%;
        padding: 4%;
        margin-left: auto;
        margin-right: auto;
      "
    >
      <li v-for="error in errors" v-bind:key="error">WARNING: {{ error }}</li>
    </ul>
    <hr />
    <h2>Add your favorite movie</h2>
    Title:
    <input type="text" v-model="newMovieTitle" />
    Year:
    <input type="text" v-model="newMovieYear" />
    Plot:
    <input type="text" v-model="newMoviePlot" />
    <button v-on:click="createMovie()">Submit</button>

    <div v-for="movie in movies" v-bind:key="movie.id" style="border-top: 1px solid #ccc; margin: 2% auto">
      <h2>{{ movie.title }}</h2>
      <p>{{ movie.year }}</p>
      <p>{{ movie.plot }}</p>
      <p style="display: inline; font-weight: 700">Genres:</p>
      <p v-for="genre in movie.genre_names" v-bind:key="genre">{{ genre }}</p>  
      <p v-for="genre in movie.genres" v-bind:key="genre.id">{{ genre.name }}</p>

      <button v-on:click="showMovie(movie)">More Info!</button>
    </div>

    <dialog id="movie-details">
      <form method="dialog">
        <h1>{{ currentMovie.title }}</h1>
        <p>
          Year:
          <input type="text" v-model="currentMovie.year" />
        </p>
        <p>
          Plot:
          <input type="text" v-model="currentMovie.plot" />
        </p>

        <ul
          v-if="errors.length"
          style="
            color: red;
            list-style: none;
            text-align: center;
            border: solid 1px red;
            width: 70%;
            padding: 4%;
            margin-left: auto;
            margin-right: auto;
          "
        >
          <li v-for="error in errors" v-bind:key="error">WARNING: {{ error }}</li>
        </ul>

        <button v-on:click="destroyMovie(currentMovie)">Delete</button>
        <button v-on:click="updateMovie(currentMovie)">Update</button>

        <button>Close</button>
      </form>
    </dialog> -->
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "The Movie - Single Page App",
      movies: [],
      genres: [],
      newMovieTitle: "",
      newMovieYear: "",
      newMoviePlot: "",
      currentMovie: "",
      errors: [],
    };
  },
  created: function () {
    this.indexMovies();
  },
  methods: {
    indexMovies: function () {
      axios.get("/api/movies").then((response) => {
        this.movies = response.data;
        console.log("All movies:", this.movies);
      });
    },
    showMovie: function (movie) {
      console.log(movie);
      this.currentMovie = movie;
      document.querySelector("#movie-details").showModal();
    },
    createMovie: function () {
      console.log("Creating a movie!");
      var params = {
        title: this.newMovieTitle,
        year: this.newMovieYear,
        plot: this.newMoviePlot,
      };
      axios
        .post("/api/movies/#top", params)
        .then((response) => {
          console.log(response.data);
          this.movies.push(response.data);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
    updateMovie: function (movie) {
      var params = {
        title: movie.title,
        plot: movie.plot,
        year: movie.year,
      };
      axios
        .patch("/api/movies/" + movie.id, params)
        .then((response) => {
          console.log("Updated Successfully", response.data);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
    destroyMovie: function (movie) {
      console.log("Deleting...");
      axios.delete("/api/movies/" + movie.id).then((response) => {
        console.log("Success!", response.data);
        var index = this.movies.indexOf(movie);
        this.movies.splice(index, 1);
      });
    },
  },
};
</script>
