<template>
  <div class="container">
    <img src="../assets/salad.jpg" alt="salad" style="width:120%;">
    
    <div class="centered">
  
  <form class='login' id='log'>
    
    <div class="row">
      <div class="col">
        <input type="text" class="form-control" placeholder="Username" v-model="usernameLogin">
      </div>
      <div class="col">
        <input type="password" class="form-control" placeholder="Password" v-model="passwordLogin">
      </div>
    </div>
    <fieldset>
        <button type="button" class="button fish btn btn-secondary"  @click.prevent="login">Log In</button>
    </fieldset>
    <div class="row">
      <div class="col">
        <input type="text" class="form-control" placeholder="First name" v-model="firstName">
      </div>
      <div class="col">
        <input type="text" class="form-control" placeholder="Last name" v-model="lastName">
      </div>
    </div>
    <div class="row">
      <div class="col">
        <input type="text" class="form-control" placeholder="Username" v-model="username">
      </div>
      <div class="col">
        <input type="password" class="form-control" placeholder="Password" v-model="password">
      </div>
    </div>
    <fieldset>
        <button type="button" class="button btn btn-secondary" @click.prevent="register">Register</button>
    </fieldset>
  </form>
  </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'Login',
  data() {
    return {
      firstName: '',
      lastName: '',
      username: '',
      password: '',
      usernameLogin: '',
      passwordLogin: '',
    }
  },
  methods: {
    async register() {
      this.error = '';
      this.errorLogin = '';
      if (!this.firstName || !this.lastName || !this.username || !this.password)
        return;
      try {
        let response = await axios.post('/api/users', {
          firstName: this.firstName,
          lastName: this.lastName,
          username: this.username,
          password: this.password,
        });
        this.$root.$data.user = response.data.user;
      } catch (error) {
        //this.error = error.response.data.message;
        this.$root.$data.user = null;
      }
    },
    async login() {
      this.error = '';
      this.errorLogin = '';
      if (!this.usernameLogin || !this.passwordLogin)
        return;
      try {
        let response = await axios.post('/api/users/login', {
          username: this.usernameLogin,
          password: this.passwordLogin,
        });
        this.$root.$data.user = response.data.user;
      } catch (error) {
        this.errorLogin = "Error: " + error.response.data.message;
        this.$root.$data.user = null;
      }
    },
  }
}

</script>

<style scoped>
.log{
  padding: 100px 200px 100px 200px !important;
}
.fish{
  margin-bottom: 50px !important;
}

.container {
  position: relative;
  text-align: center;

    width: 100%;
    padding-right: 0px;
    padding-left: 0px;
    margin-right: none;
    margin-left: none;
    align-self: flex-start;
    float: left;
}
.centered {
  position: absolute;
  top: 40%;
  left: 60%;
  transform: translate(-50%, -50%);
  background-color: rgb(194, 209, 209);
  padding:30px;
  border-radius: 25px;
}
.button{
  margin: 10px;
}
</style>
