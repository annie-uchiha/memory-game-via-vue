<template>
  <div id="app">
    <GameBoard />
  </div>
</template>

<script>
import GameBoard from "./components/GameBoard.vue";

export default {
  name: "App",
  components: {
    GameBoard,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: rgba(190, 75, 9, 0.801);
  margin-top: 60px;
}
</style>
