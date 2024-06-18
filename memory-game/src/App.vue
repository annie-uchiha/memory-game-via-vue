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

body {
  background-color: black;
  background-image: url("./assets/2ac217d1943ba93dd7de989ecd9e5f35.jpg"),
    url("./assets/9183.png");
  background-repeat: no-repeat, no-repeat;
  background-size: contain, cover;
}
</style>
