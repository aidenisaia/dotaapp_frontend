<template>
  <div class="home">
    {{ builds }}
    </hr>
    <div class="item_list">
      <template v-for="item in items">
        <img class="image" @error="imageLoadError(item)" v-if="item.display === true" :src=item.url >
      </template>
    </div>
    </hr>
    {{ heroes }}
  </div>
</template>

<style>
.item_list {
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
      builds: [],
      items: [],
      heroes: [],
    };
  },
  created: function () {
    this.indexBuilds();
  },
  methods: {
    indexBuilds: function () {
      axios.get("/builds").then((response) => {
        console.log(response.data);
        this.builds = response.data["builds"];
        this.items = response.data["items"];
        this.heroes = response.data["heroes"];
        this.items.forEach(function (item) {
          item.display = true;
        });
      });
    },
    imageLoadError(item) {
      console.log("Image failed to load");
      item.display = false;
    },
  },
};
</script>
