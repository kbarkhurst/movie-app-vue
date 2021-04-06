<template>
  <div class="container my-3">
    <div class="row">
      <div class="col-8 mx-auto">
        <div class="movies-new">
          <form v-on:submit.prevent="createMovie()">
            <h1>Add your favorite movie</h1>
            <ul>
              <li class="text-danger" v-for="error in errors" v-bind:key="error">
                {{ error }}
              </li>
            </ul>
            <div class="form-group">
              <label>Title:</label>
              <input type="text" class="form-control" v-model="title" />
            </div>
            <div class="form-group">
              <label>Year:</label>
              <input type="text" class="form-control" v-model="year" />
            </div>
            <div class="form-group">
              <label>Plot:</label>
              <input type="text" class="form-control" v-model="plot" />
            </div>

            <input type="submit" class="btn btn-primary my-2" value="Submit" />
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      title: "",
      year: "",
      plot: "",
      errors: [],
    };
  },
  created: function () {},
  methods: {
    createMovie: function () {
      console.log("Creating your new movie.");
      var params = {
        title: this.title,
        year: this.year,
        plot: this.plot,
      };
      axios
        .post("/api/movies/", params)
        .then((response) => {
          console.log(response.data);
          this.$router.push("/movies#top");
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
