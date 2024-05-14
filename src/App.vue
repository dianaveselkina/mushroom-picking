<template>
  <div class="app">
    <GameBoard />
  </div>
</template>
<script>
import GameBoard from './components/GameBoard.vue';
export default {
  components: {
    GameBoard,
  },

  data() {
    return {};
  },
  methods: {},
};
</script>
<style>
.app {
  max-width: 2600px;
  margin: 0 auto;
  background-color: #ffffff;
  border-radius: 6px;
}
</style>
