<template>
  <div class="home">
    {{ builds }}
    </hr>
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
  },
};
</script>
