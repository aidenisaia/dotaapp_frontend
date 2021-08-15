<template>
  <div class="home">
    {{ builds }}
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      builds: [],
      items: [],
      heroes: [],
    };
  },
  created: function () {
    this.indexBuilds();
  },
  methods: {
    indexBuilds: function () {
      axios.get("/builds").then((response) => {
        console.log(response.data);
        this.builds = response.data["builds"];
      });
    },
  },
};
</script>
