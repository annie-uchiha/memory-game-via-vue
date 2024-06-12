<template>
  <div class="card" @click="handleClick" :class="{ flipped: isFlipped }">
    <div class="card-front"></div>
    <div class="card-back">
      <img :src="require(`@/cardGifs/${image}`)" alt="Card Image" />
    </div>
  </div>
</template>

<script>
export default {
  name: "MemoryCard",
  props: {
    image: String,
    id: Number,
    flipped: Boolean,
  },
  data() {
    return {
      isFlipped: this.flipped,
    };
  },
  watch: {
    flipped(newVal) {
      this.isFlipped = newVal;
    },
  },
  methods: {
    handleClick() {
      if (!this.isFlipped) {
        this.$emit("card-flipped", this.id);
      }
    },
  },
};
</script>

<style scoped>
.card {
  width: 220px;
  height: 220px;
  perspective: 1000px;
  cursor: pointer;
  position: relative;
}

.card-front,
.card-back {
  width: 100%;
  height: 100%;
  position: absolute;
  backface-visibility: hidden;
  transition: transform 0.6s;
}

.card-front {
  background: linear-gradient(to right, rgba(190, 75, 9, 0.801), #ff9f06);
  transform: rotateY(0deg);
}

.card-back {
  transform: rotateY(180deg);
}

.card.flipped .card-front {
  transform: rotateY(180deg);
}

.card.flipped .card-back {
  transform: rotateY(0deg);
}

.card-back img {
  max-width: 100%;
  max-height: 100%;
}
</style>
