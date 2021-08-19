<template>
  <div class="home">
    <div class="hero_list">
      <span v-for="hero in heroes" v-if=hero.display>
        <img class="image"
          :src=hero.url
          v-on:click="addHeroToBuild(hero)"  @error="imageLoadError(hero)" 
          >
      </span>
    </div>
    <hr>
    <div class="build">
      <form v-on:submit.prevent="submit()">
        <input type="text" v-model="timing" placeholder="Timing">
        <br>
        <img class="image" :src=this.herochoice.url>
        <br>
        <span class="item_list" v-for="item in itemchoices">
          <img class="image" v-on:click="removeItem(item)" :src=item.url>
        </span>
        <input type="submit" value="Submit" />
      </form>
    </div>
    <hr>
    <div class="item_list">
      <span v-for="item in items" v-if=item.display>
        <img class="image"
          @error="imageLoadError(item)"
          :src=item.url
          v-on:click="addItemToBuild(item)"
          >
      </span>
    </div>
  </div>
</template>

<style>
.item_list,
.hero_list {
  width: 100%;
  display: inline-block;
  flex-flow: row nowrap;
}
.image {
  max-width: 70px;
}
.home {
  padding-top: 6em;
}
</style>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      heroes: [],
      items: [],
      herochoice: "",
      itemchoices: [],
      timing: "",
      errors: [],
      newBuildParams: {},
    };
  },
  created: function () {
    this.buildData();
  },
  methods: {
    buildData: function () {
      axios.get("/builds").then((response) => {
        console.log(response.data);
        this.items = response.data["response_items"];
        this.heroes = response.data["heroes"];
        console.log(response.data["heroes"]);
        this.items.forEach(function (item) {
          item.display = true;
        });
        this.heroes.forEach(function (hero) {
          hero.display = true;
        });
      });
    },
    imageLoadError(item) {
      console.log("Image failed to load");
      item.display = false;
    },
    addItemToBuild: function (item) {
      console.log(item);
      this.itemchoices.push(item);
    },
    addHeroToBuild: function (hero) {
      console.log(hero);
      this.herochoice = hero;
    },
    removeItem: function (item) {
      this.itemchoices.splice(this.itemchoices.indexOf(item), 1);
    },
    submit: function () {
      this.newBuildParams.timing = this.timing;
      this.newBuildParams.hero_name = this.herochoice.name;
      this.newBuildParams.hero_url = this.herochoice.url;
      this.newBuildParams.itemchoices = this.itemchoices;
      console.log(this.newBuildParams);
      axios
        .post("/builds", this.newBuildParams)
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