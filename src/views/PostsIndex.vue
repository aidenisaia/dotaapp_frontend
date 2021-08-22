<template>
  <div class="home">
    <h1>New Post</h1>
    <form v-on:submit.prevent="submitPost()">
      <p><input class="input_text" type="text" v-model="newPostParams.title" placeholder="Title" /></p>
      <p><input class="input_text" type="text" v-model="newPostParams.body" placeholder="Body"></p>
      <p><input class="input_text" type="text" v-model="newPostParams.build_id" placeholder="Build Id"></p>
      <input type="submit" value="Submit" />
    </form>
    <br>
    <hr>
    <div v-for="post in posts">
      <router-link v-bind:to="`/posts/${post.id}`">
        <h2 class="grow">{{ post.title }}</h2>
      </router-link>
      <h3>- {{ users.find(x => x.id === post.user_id).username }}</h3>
      <br>
      <p class="body">{{ post.body }}</p>
      <div v-for="build in builds" v-if="post.build_id === build.id">
        <p class="build_title">{{ build.timing }} {{ build.hero_name }} build -
        {{ users.find(x => x.id === build.user_id).username }}</p>
        <p><img :src=build.hero_url></p>
        <span v-for="item in items">
          <span v-if="item.build_id === build.id">
            <img class="image" :src=item.url>
          </span>
        </span>
      </div>
      <br>
      <div v-show="isShow" v-for="comment in comments">
        <p class="comment" v-if="comment.post_id === post.id">
          {{ comment.body }} - {{ users.find(x => x.id === comment.user_id).username }}
          <br>
        </p>
      </div>
      <br>
      <button @click="isShow = !isShow" type="button" class="btn btn-secondary">Comments</button>
      <hr>
    </div>
  </div>
</template>

<style>
h1 {
  font-size: 30px;
  color: whitesmoke;
}

h3 {
  margin-top: 0;
  color: #405a74;
}
h2 {
  margin-bottom: 0;
  color: rgb(151, 217, 255);
  font-weight: bold;
}
.comment {
  color: #94d4ff;
  padding-top: 1em;
}
.body {
  color: #34aeff;
  margin-bottom: 1em;
  font-size: 14pt;
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
      isShow: false,
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
