<template>
  <div class="board">
    <div class="basket"></div>
  </div>
</template>
<script>
export default {
  data() {
    return {};
  },
  methods: {},
};
</script>
<style>
.board {
  margin: 100px auto;
  padding: 50px 0;
  width: 2400px;
  height: 2400px;
  background-color: rgb(29, 56, 26);
}
.basket {
  background-image: url(../img/basket.jpg);
  z-index: 5;
  background-repeat: no-repeat;
  background-size: cover;
  width: 250px;
  height: 250px;
  margin: 2000px auto 0;
}
</style>
