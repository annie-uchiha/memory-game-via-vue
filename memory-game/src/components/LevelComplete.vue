<template>
  <div class="overlay">
    <img :src="gif" alt="Level Complete" />
    <button @click="$emit('next-level')">Next Level</button>
  </div>
</template>

<script>
export default {
  props: {
    gif: String,
  },
};
</script>

<style scoped>
.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.overlay img {
  max-width: 80%;
  max-height: 80%;
}

.overlay button {
  margin-top: 20px;
  padding: 10px 20px;
  font-size: 16px;
}
</style>
