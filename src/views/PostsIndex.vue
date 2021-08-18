<template>
  <div class="home">
    <form v-on:submit.prevent="submitPost()">
      <input type="text" v-model="newPostParams.title" placeholder="Title" />
      <br>
      <input type="text" v-model="newPostParams.body" placeholder="Body">
      <br>
      <input type="text" v-model="newPostParams.build_id" placeholder="Build Id">
      <input type="submit" value="Submit" />
    </form>
    <br>
    <hr>
    <div v-for="post in posts">
      <router-link v-bind:to="`/posts/${post.id}`">
        <h2>{{ post.title }}</h2>
      </router-link>
      <h3>{{ users.find(x => x.id === post.user_id).username }}</h3>
      <p>{{ post.body }}</p>
      <div v-for="build in builds" v-if="post.build_id === build.id">
        {{ build.timing }} {{ build.hero_name }} build:
        {{ users.find(x => x.id === build.user_id).username }}
        <br>
        <p><img :src=build.hero_url></p>
        <span v-for="item in items">
          <span v-if="item.build_id === build.id">
            <img :src=item.url>
          </span>
        </span>
      </div>
      <br>
      <div v-for="comment in comments">
        <p v-if="comment.post_id === post.id">
          {{ comment.body }}
          <br>
          - {{ users.find(x => x.id === comment.user_id).username }}
        </p>
      </div>
      <hr>
    </div>
  </div>
</template>

<style>
h3 {
  margin-top: 0;
}
h2 {
  margin-bottom: 0;
}
</style>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      posts: [],
      comments: [],
      users: [],
      builds: [],
      items: [],
      newPostParams: {},
    };
  },
  created: function () {
    this.indexPosts();
    this.indexComments();
    this.indexUsers();
  },
  methods: {
    indexPosts: function () {
      axios.get("/posts").then((response) => {
        console.log(response.data["posts"]);
        this.posts = response.data["posts"];
        console.log(response.data["builds"]);
        this.builds = response.data["builds"];
        console.log(response.data["items"]);
        this.items = response.data["items"];
      });
    },
    indexComments: function () {
      axios.get("/comments").then((response) => {
        console.log(response.data);
        this.comments = response.data;
      });
    },
    indexUsers: function () {
      axios.get("/users").then((response) => {
        console.log(response.data);
        this.users = response.data;
      });
    },
    submitPost: function () {
      if (this.newPostParams.build_id) {
        this.newPostParams.build_id = parseInt(this.newPostParams.build_id, 10);
      } else {
        this.newPostParams.build_id = null;
      }
      axios
        .post("/posts", this.newPostParams)
        .then((response) => {
          console.log(response.data);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
      location.reload();
    },
  },
};
</script>
