<template>
    <div>
        <div class="w3-sidebar w3-bar-block w3-collapse w3-card" style="width:200px;" id="mySidebar">
        <button class="w3-bar-item w3-button w3-hide-large"
        onclick="w3_close()">Close &times;</button>
        <a href="#" class="w3-bar-item w3-button">Link 1</a>
        <a href="#" class="w3-bar-item w3-button">Link 2</a>
        <a href="#" class="w3-bar-item w3-button">Link 3</a>
        </div>

        <div class="w3-main" style="margin-left:200px"></div>
    </div>
</template>


<script>
import axios from "axios";

export default {
  name: "Main",
  props: {
    token: {
      type: String
    }
  },
  data() {
    return {};
  },
  methods: {}
};
</script>