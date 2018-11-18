<template>
  <div id="app">
    <header v-if="!token">
      <span>Drivepack App</span>
    </header>
    <main>
        <Login  v-if="!token"></Login>
        <Main v-if="token" :token="token"></Main>
    </main>
  </div>
</template>

<script>
import Login from './components/Login';
import Main from './components/Main';

import EventBus from "./classes/eventBus.js";

export default {
  name: 'app',
  components: {
      Login,
      Main
  },
  data() {
    return {
      token: null
    };
  },
  created: function() {
      this.backendUrl = "localhost:5000"

    EventBus.$on('loggedIn', message => {
        console.log("Logged in token: " + message)
        this.token = message
    })
  }
}
</script>

<style>
body {
  margin: 0;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

main {
  text-align: center;
}

header {
  margin: 0;
  height: 56px;
  padding: 0 16px 0 24px;
  background-color: #35495E;
  color: #ffffff;
}

header span {
  display: block;
  position: relative;
  font-size: 20px;
  line-height: 1;
  letter-spacing: .02em;
  font-weight: 400;
  box-sizing: border-box;
  padding-top: 16px;
}
</style>
