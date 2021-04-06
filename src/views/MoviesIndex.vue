<template>
  <div class="container my-3">
    <div class="home">
      <div>
        <div data-masonry='{"percentPosition": true }' class="row row-cols-1 row-cols-sm-2 row-cols-lg-3 g-4">
          <div class="col" v-for="movie in movies" v-bind:key="movie.id">
            <router-link title="More about this Movie" v-bind:to="`movies/${movie.id}`">
              <div class="card">
                <div class="row g-0">
                  <div v-if="movie.image" class="col-md-4 d-flex align-items-center">
                    <img v-bind:src="movie.image" class="card-img-top" alt="movie.title" />
                  </div>

                  <div class="col">
                    <div class="card-body">
                      <h5 class="card-title">{{ movie.title }}</h5>
                      <p class="card-text">
                        {{ movie.plot }}
                      </p>
                      <small>{{ movie.year }}</small>
                    </div>
                  </div>

                  <div class="card-footer">
                    <small class="text-muted">
                      <span v-for="genre in movie.genres" v-bind:key="genre.id">{{ genre.name }}</span>
                    </small>
                  </div>
                </div>
              </div>
            </router-link>
          </div>
        </div>
        <!-- <div class="card">
                <img v-if="movie.image" v-bind:src="movie.image" class="card-img-top" alt="movie.title" />
                <div class="card-body">
                  <h5 class="card-title">{{ movie.title }}</h5>
                  <p class="card-text">
                    {{ movie.plot }}
                  </p>
                  <small>{{ movie.year }}</small>
                </div>
              </div> -->

        <!-- <div v-for="movie in movies" v-bind:key="movie.id">
          <router-link title="More about this Movie" v-bind:to="`movies/${movie.id}`">
            <h3>{{ movie.title }}</h3>
            <p>{{ movie.plot }}</p>
            <p>{{ movie.year }}</p>
            <hr />
          </router-link>
        </div> -->
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
