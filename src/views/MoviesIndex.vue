<template>
  <div class="container my-3">
    <div class="home">
      <!-- views/MoviesIndex.vue -->

      <!-- <datalist id="titles">
          <option v-for="movie in movies" v-bind:key="movie.id">{{ movie.title }}</option>
        </datalist> -->
      <!-- Search by name:
        <input v-model="titleFilter" list="titles" />
        <datalist id="titles">
          <option v-for="movie in movies" v-bind:key="movie.id">{{ movie.title }}</option>
        </datalist> 
      </div>-->
      <!-- Bar containing all sort inputs -->
      <div>
        <div class="row py-3">
          <div class="col-6">
            Search by title:
            <input v-model="searchValue" placeholder="Search Movies" id="search-input" />
            <!-- <datalist id="titles">
              <option v-for="movie in filteredMovies" v-bind:key="movie.id">{{ movie.title }}</option>
            </datalist> -->
            <!-- <input v-model="titleFilter" list="titles" />
            <datalist id="titles">
              <option v-for="movie in filteredMovies" v-bind:key="movie.id">{{ movie.title }}</option>
            </datalist> -->
          </div>
          <div class="col-6">
            <select name="sortBy" id="select" v-model="sortBy">
              <option value="alphabetically">Alphabetically</option>
              <option value="year">Year</option>
            </select>
            <button v-on:click="ascending = !ascending" class="sort-button">
              <i v-if="ascending">ASC</i>
              <i v-else>DSC</i>
            </button>
          </div>
        </div>
        <div
          is="transition-group"
          data-masonry='{"percentPosition": true }'
          class="row row-cols-1 row-cols-sm-2 row-cols-lg-3 g-4"
          appear
          enter-active-class="animated fadeIn"
          leave-active-class="animated fadeOut"
        >
          <div v-bind:key="movie.id" v-for="movie in filteredMovies">
            <!-- <div v-for="movie in orderBy(filterBy(movies, titleFilter, 'title'), 'title', -1)" v-bind:key="movie.id"> -->
            <!-- <div v-bind:key="movie.id" v-for="movie in filterBy(movies, titleFilter, 'title')"> -->
            <!-- <div class="col" v-for="movie in movies" v-bind:key="movie.id"> -->
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
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function () {
    return {
      movies: [],
      movie: {},
      titleFilter: "",
      ascending: true,
      sortBy: "alphabetically",
      searchValue: "",
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
  computed: {
    filteredMovies() {
      let tempMovies = this.movies;

      if (this.searchValue != "" && this.searchValue) {
        tempMovies = tempMovies.filter((movie) => {
          return movie.title.toUpperCase().includes(this.searchValue.toUpperCase());
        });
      }

      // Sort by alphabetical order
      tempMovies = tempMovies.sort((a, b) => {
        if (this.sortBy == "alphabetically") {
          let fa = a.title.toLowerCase(),
            fb = b.title.toLowerCase();

          if (fa < fb) {
            return -1;
          }
          if (fa > fb) {
            return 1;
          }
          return 0;

          // Sort by year
        } else if (this.sortBy == "year") {
          return a.year - b.year;
        }
      });

      // Show sorted array in descending or ascending order
      if (!this.ascending) {
        tempMovies.reverse();
      }

      return tempMovies;
    },
  },
};
</script>
<style scoped>
#sort-bar {
  width: 80%;
  margin-left: 10px;
  background-color: #f2e0c1;
  display: flex;
  flex-wrap: wrap;
  padding: 10px;
}
#ascending-icon {
  height: 30px;
  width: 30px;
  background-color: red;
}
.sort-button {
  background-color: rgba(0, 0, 0, 1);
  border: none;
  height: 50px;
  width: 50px;
}
.sort-button i {
  color: white;
}

#sort-label {
  font-size: 12px;
}
</style>
