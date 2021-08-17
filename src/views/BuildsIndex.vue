<template>
  <div class="home">
    <div v-for="build in builds" class="build">
      {{ build.timing }} {{ build.hero_name }} build:
      {{ users.find(x => x.id === build.user_id).username }}
      <br>
      <p><img :src=build.hero_url></p>
      <span v-for="item in items">
        <span v-if="item.build_id === build.id">
          <img :src=item.url>
        </span>
      </span>
      <hr>
    </div>
  </div>
</template>

<style>
.build {
  margin: 2em;
}
p {
  padding: 0;
  margin-bottom: 0;
}
</style>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      builds: [],
      items: [],
      heroes: [],
      users: [],
      responseItems: [],
    };
  },
  created: function () {
    this.indexBuilds();
    this.indexUsers();
    this.indexItems();
  },
  methods: {
    indexBuilds: function () {
      axios.get("/builds").then((response) => {
        console.log(response.data["builds"]);
        this.builds = response.data["builds"];
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
  },
};
</script>
