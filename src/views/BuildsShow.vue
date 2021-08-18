<template>
  <div class="build_show">
    {{ build.timing }} {{ build.hero_name }} build:
    {{ users.find(x => x.id === build.user_id).username }}
    <br>
    <p><img :src=build.hero_url></p>
    <span v-for="item in items">
      <span v-if="item.build_id === build.id">
        <img :src=item.url>
      </span>
    </span>
    <br>
  <button v-on:click="deleteBuild()">Delete Build</button>
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
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>