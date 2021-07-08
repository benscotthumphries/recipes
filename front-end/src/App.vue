<template>
  <div id="app">
    <Login v-if='!user' />
    <div v-if='user' id="nav">
      <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
      <div class="navbar-brand" >YUM Recipies</div>
      <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarTogglerDemo02" aria-controls="navbarTogglerDemo02" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarTogglerDemo02">
        <ul class="navbar-nav mr-auto mt-2 mt-lg-0">
          <li class="nav-item ">
            <router-link  class="nav-link" to="/">Search</router-link>
          </li>
          <li class="nav-item">
            <router-link  class="nav-link" to="/about">My Recipies</router-link>
          </li>
        </ul>
        <div class="menu">
          <p><a><i class="fas fa-image"></i></a></p>
          <h2 class='user-name'>{{user.firstName}} {{user.lastName}} <button @click="logout" type="button" class="btn btn-outline-primary">Log Out</button></h2>
        </div>
      </div>
    </nav>
    </div>
    <router-view v-if="user" />
    <div class='footer'>
      <p>I spent 30 hours | <a href="https://github.com/benscotthumphries/Hang">GitHub</a></p>
    </div>
  </div>
</template>

<script>
import Login from '@/components/Login.vue';
import axios from 'axios';
export default {
  name: 'dashboard',
  components: {
    Login,
  },
  async created() {
    try {
      let response = await axios.get('/api/users');
      this.$root.$data.user = response.data.user;
    } catch (error) {
      this.$root.$data.user = null;
    }
  },
  computed: {
    user() {
      return this.$root.$data.user;
    }
  },
  methods: {
    async logout() {
      try {
        await axios.delete("/api/users");
        this.$root.$data.user = null;
      } catch (error) {
        this.$root.$data.user = null;
      }
    },
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
.btn-outline-success{
  color: white;
  border-color: #427fb9;
  background-color: #427fb9;
}
#nav a {
  font-weight: bold;
  color: white;
}
#nav a.router-link-exact-active {
  color: #427fb9;
}
.user-name{
  color: aliceblue;
  font-size: 16px;
}
</style>
