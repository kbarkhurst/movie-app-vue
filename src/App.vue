<template>
  <div id="app">
    <header>
      <nav class="navbar navbar-expand-sm navbar-dark bg-primary">
        <div class="container">
          <a class="navbar-brand" href="/">
            <i class="bi bi-camera-reels-fill"></i> <i class="bi bi-telephone-fill"></i>

            Movie Phone
          </a>
          <ul class="navbar-nav mr-auto">
            <li class="nav-item active">
              <router-link class="nav-link" to="/movies">Explore our Collection</router-link>
            </li>
            <li class="nav-item" v-if="isLoggedIn()">
              <router-link class="nav-link" to="/movies/new">Add Movie</router-link>
            </li>
          </ul>
          <ul class="navbar-nav ml-auto">
            <li class="nav-item" v-if="!isLoggedIn()">
              <router-link class="nav-link" to="/login">Login</router-link>
            </li>
            <li class="nav-item" v-if="!isLoggedIn()">
              <router-link class="btn btn-warning" to="/signup">Signup</router-link>
            </li>
            <li class="nav-item" v-if="isLoggedIn()">
              <router-link class="nav-link" to="/logout">Logout</router-link>
            </li>
          </ul>
        </div>
      </nav>
    </header>
    <main></main>
    <router-view />
  </div>
</template>

<script>
export default {
  methods: {
    isLoggedIn: function () {
      return localStorage.getItem("jwt");
    },
    getUserId: function () {
      return localStorage.getItem("user_id");
    },
  },
};
</script>
