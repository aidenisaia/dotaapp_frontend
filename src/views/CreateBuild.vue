<template>
  <div class="home">
    <div class="hero_list">
      <button v-for="hero in heroes">
        <img class="image"
          :src=hero.url
          v-on:click="addHeroToBuild(hero)"  @error="imageLoadError(hero)" 
          v-if=hero.url>
      </button>
    </div>
    <hr>
    <div class="build">
      <img class="image" :src=this.herochoice.url>
      <div v-for="item in itemchoices">
        <img class="image" v-on:click="removeItem(item)":src=item.url>
      </div>
    </div>
    <hr>
    <div class="item_list">
      <button v-for="(item, index) in items">
        <img class="image"
          :src=item.url
          v-on:click="addItemToBuild(item)"  @error="imageLoadError(item)" 
          v-if=item.url>
      </button>
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
    };
  },
  created: function () {
    this.buildData();
  },
  methods: {
    buildData: function () {
      axios.get("/items").then((response) => {
        console.log(response.data);
        this.builds = response.data["builds"];
        this.items = response.data["items"];
        this.heroes = response.data["heroes"];
        // this.items.forEach(function (item) {
        //   item.display = true;
        // });
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
  },
};
</script>