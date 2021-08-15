<template>
  <div class="home">
    <div v-for="post in posts">
      <h2>{{ post.title }}</h2>
      <h3>{{ users.find(x => x.id === post.user_id).username }}</h3>
      <p>{{ post.body }}</p>
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
        console.log(response.data);
        this.posts = response.data;
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
  },
};
</script>
