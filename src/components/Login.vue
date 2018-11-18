<template>
    <div class="w3-container">
        <form class="login w3-container" @submit.prevent="login">
            <div class="w3-section">
                <h1>Sign in</h1>
                <div>
                    <label class="w3-left"><b>Email</b></label><br>
                    <input required v-model="email" type="text" placeholder="example@gmail.com" class="w3-input w3-border w3-margin-bottom"/>
                </div>
                <div>
                    <label class="w3-left"><b>Password</b></label><br>
                    <input required v-model="password" type="password" placeholder="Password" class="w3-input w3-border w3-margin-bottom"/>
                </div>
                <button type="submit" class="w3-button w3-block w3-green w3-section w3-padding">Login</button>
                <button v-on:click='showRegister = !showRegister'  class="w3-button w3-block w3-blue w3-section w3-padding">Register</button>
            </div>
        </form>
        <Register v-if="showRegister" @toggleRegister="toggleRegister"></Register>
        <div v-if="loginSuccesfull" class="w3-panel w3-pale-green w3-border">
            <h3>Login succesfull!</h3>
        </div>
        <div v-if="errorLogin" class="w3-panel w3-pale-red w3-border">
            <h3>Bad credintials or account don't exists!</h3>
        </div>
    </div>
</template>

<script>
import Register from "./Register";
import axios from 'axios';
import EventBus from "../classes/eventBus.js";

export default {
  name: "Login",
  components: {
    Register
  },
  onCreated() {},
  data() {
    return {
      password: "",
      email: "",
      showRegister: false,
      loginSuccesfull: false,
      errorLogin: false
    };
  },
  methods: {
    login: function() {
      const { email, password } = this;
      console.log(email);
      console.log(password);
      axios
        .post("http://localhost:5000/login", {
          email: email,
          password: password
        })
        .then(response => {
          this.loginSuccesfull = true;
          this.errorLogin = false;
          let token = response.data.token

          setTimeout( () => {
              EventBus.$emit('loggedIn', token)
          }, 1000)
        })
        .catch(e => {
          this.errorLogin = true;
          this.loginSuccesfull = false;
          //this.errors.push(e);
        });
    },
    toggleRegister: function() {
      this.showRegister = !this.showRegister;
    }
  }
};
</script>