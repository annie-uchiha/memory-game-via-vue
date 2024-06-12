<template>
  <div class="card" @click="flipCard" :class="{ flipped: isFlipped }">
    <div class="card-front"></div>
    <div class="card-back">
      <img :src="image" alt="Card Image" />
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
    flipCard() {
      if (!this.isFlipped) {
        this.$emit("card-flipped", this.id);
      }
    },
  },
};
</script>

<style scoped>
.card {
  width: 320px;
  height: 250px;
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
  background: -webkit-gradient(
    linear,
    left top,
    right top,
    from(rgba(190, 75, 9, 0.801)),
    to(#ff9f06)
  );
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
