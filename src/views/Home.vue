<template>
  <div class="home front_page">
  </div>
</template>

<style>
.front_page {
  background: url("../../public/images/wraith.jpeg") no-repeat center center fixed;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;

  min-height: 100%;
  min-width: 1024px;

  /* Set up proportionate scaling */
  width: 100%;
  height: auto;

  /* Set up positioning */
  position: fixed;
  top: 0;
  left: 0;
}

@media screen and (max-width: 1024px) {
  /* Specific to this particular image */
  img.bg {
    left: 50%;
    margin-left: -512px; /* 50% */
  }
}
</style>

<script>
// $('html').css({'background': 'url(' images[Math.floor(Math.random() * images.length)] + ')'});
export default {
  data: function () {
    return {};
  },
  created: function () {},
  methods: {},
};
</script>
