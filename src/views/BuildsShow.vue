<template>
  <div class="home">
    <p class="build_title">
    {{ build.timing }} {{ build.hero_name }} build:
    {{ users.find(x => x.id === build.user_id).username }}
    </p>
    <p><img class="image" :src=build.hero_url></p>
    <span v-for="item in items">
      <span v-if="item.build_id === build.id">
        <img class="image" :src=item.url>
      </span>
    </span>
    <br>
    <button v-on:click="deleteBuild()">Delete Build</button>
    <br>
    <br>
    <p class="error">{{ error }}</p>
    <!-- <ul>
      <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
    </ul> -->
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      build: {},
      users: [],
      items: [],
      error: "",
    };
  },
  created: function () {
    this.buildShow();
    this.indexUsers();
    this.indexItems();
  },
  methods: {
    buildShow: function () {
      console.log("getting a single build");
      console.log(this.$route);
      // get data from rails
      axios.get(`/builds/${this.$route.params.id}`).then((response) => {
        console.log(response.data);
        this.build = response.data;
      });
    },
    indexUsers: function () {
      axios.get("/users").then((response) => {
        console.log(response.data);
        this.users = response.data;
      });
    },
    indexItems: function () {
      axios.get("/items").then((response) => {
        console.log(response.data["items"]);
        this.items = response.data["items"];
        this.responseItems = response.data["response_items"];
        console.log(this.responseItems);
      });
    },
    deleteBuild: function () {
      axios
        .delete(`/builds/${this.$route.params.id}`)
        .then((response) => {
          console.log(response.data);
          this.$router.push("/builds");
        })
        .catch((error) => {
          console.log(error.response);
          this.error = error.response.request.statusText;
        });
    },
  },
};
</script>