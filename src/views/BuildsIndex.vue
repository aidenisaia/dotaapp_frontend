<template>
  <div class="home">
    <div v-for="build in builds">
      <router-link v-bind:to="`/builds/${build.id}`">
      <p class="build_title grow">{{ build.timing }} {{ build.hero_name }} build -
      {{ users.find(x => x.id === build.user_id).username }}
      <br>
      Build ID: {{ build.id }}
      </p>
      </router-link>
      <br>
      <p><img class="image" :src=build.hero_url></p>
      <span v-for="item in items">
        <span v-if="item.build_id === build.id">
          <img class="image" :src=item.url>
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
.build_title {
  color: rgb(218, 241, 255);
  font-size: 13pt;
  margin-top: 2em;
  margin-bottom: 1em;
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
