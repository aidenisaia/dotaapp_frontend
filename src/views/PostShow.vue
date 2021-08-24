<template>
  <div class="home">
    <h2>{{ post.title }}</h2>
    <h3>- {{ users.find(x => x.id === post.user_id).username }}</h3>
    <p class="body">{{ post.body }}</p>
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
      <p class="comment" v-if="comment.post_id === post.id">
        {{ comment.body }}
        - {{ users.find(x => x.id === comment.user_id).username }}
      </p>
    </div>
    <form v-on:submit.prevent="submitComment()">
      <input class="input_text" type="text" v-model="newCommentParams.body" placeholder="Comment" />
      <input type="submit" value="Submit" />
    </form>
    <button v-on:click="deletePost()">Delete Post</button>
    <br>
    <br>
    <p class="error">{{error}}</p>
    <hr>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      post: {},
      comments: [],
      users: [],
      builds: [],
      items: [],
      newCommentParams: {},
      error: "",
      errors: [],
    };
  },
  created: function () {
    this.postShow();
    this.indexComments();
    this.indexUsers();
    this.indexPosts();
  },
  methods: {
    postShow: function () {
      console.log("getting a single post");
      console.log(this.$route);
      // get data from rails
      axios.get(`/posts/${this.$route.params.id}`).then((response) => {
        console.log(response.data);
        this.post = response.data;
      });
    },
    indexPosts: function () {
      axios.get("/posts").then((response) => {
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
    submitComment: function () {
      this.newCommentParams.post_id = this.post.id;
      axios
        .post("/comments", this.newCommentParams)
        .then((response) => {
          console.log(response.data);
          location.reload();
        })
        .catch(() => {
          this.error = "Must be logged in";
        });
    },
    deletePost: function () {
      axios
        .delete(`/posts/${this.$route.params.id}`)
        .then((response) => {
          console.log(response.data);
          this.$router.push("/posts");
        })
        .catch((error) => {
          this.error = error.response.request.statusText;
        });
    },
  },
};
</script>
