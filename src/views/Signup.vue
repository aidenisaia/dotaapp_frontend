<template>
  <div class="signup">
    <form v-on:submit.prevent="submit()">
      <h1>Signup</h1>
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      <div>
        <label>Username:</label>
        <input class="input_text" type="text" v-model="newUserParams.username" />
        <br>
      </div>
      <div>
        <label>Email:</label>
        <input class="input_text" type="email" v-model="newUserParams.email" />
        <br>
      </div>
      <div>
        <label>Password:</label>
        <input class="input_text" type="password" v-model="newUserParams.password" />
        <br>
      </div>
      <div>
        <label>Password confirmation:</label>
        <input class="input_text" type="password" v-model="newUserParams.password_confirmation" />
        <br>
      </div>
      <input type="submit" value="Submit" />
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      newUserParams: {},
      errors: [],
    };
  },
  methods: {
    submit: function () {
      axios
        .post("/users", this.newUserParams)
        .then((response) => {
          console.log(response.data);
          this.$router.push("/login");
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
