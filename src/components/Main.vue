
<template>
    <div id="menu">
        <header>
            <span class="w3-left">Menu</span>
        </header>
        <div v-if="!addPackageFlag" id="main">
            <h2> Welcome {{ userName }}!</h2>
            <div id="button-container">
                <button v-on:click="addPackageFlag = !addPackageFlag" class="w3-button w3-block w3-green w3-section w3-padding">Add Package</button>
                <button v-on:click="addPackageFlag = !addPackageFlag" class="w3-button w3-block w3-green w3-section w3-padding">Your Packages</button>
                <button v-on:click="addPackageFlag = !addPackageFlag" class="w3-button w3-block w3-green w3-section w3-padding">Settings</button>
            </div>
        </div>
        <AddPackage v-if="addPackageFlag" @toggleAddPackage="addPackageFlag = !addPackageFlag"></AddPackage>
    </div>
</template>

<style>
#button-container {
  margin: 30px;
}

#menu {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

header {
  margin: 0;
  height: 56px;
  padding: 0 16px 0 24px;
  background-color: #35495e;
  color: #ffffff;
}
</style>



<script>
import axios from "axios";
import AddPackage from './AddPackage';

export default {
  name: "Main",
  components: {
      AddPackage
  },
  props: {
    token: {
      type: String
    }
  },
  data() {
    return {
      user: null,
      userName: '',
      addPackageFlag: false
    };
  },
  created: function() {
    let config = {
      headers: {
        "x-access-token": this.token
      }
    };
    axios
      .get("http://localhost:5000/users/me", config)
      .then(response => {
        this.user = response.data
        this.userName = response.data.name
      })
      .catch(e => { console.log(e)});
  },
  methods: {

  }
};
</script>