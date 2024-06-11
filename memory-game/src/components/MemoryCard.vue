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
    image: {
      type: String,
      required: true,
    },
    id: {
      type: Number,
      required: true,
    },
    flipped: {
      type: Boolean,
      required: true,
    },
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
        // Prevent flipping back by the card itself
        this.isFlipped = !this.isFlipped;
        this.$emit("card-flipped", this.id);
      }
    },
  },
};
</script>

<style scoped>
.card {
  width: 100px;
  height: 150px;
  position: relative;
  perspective: 1000px;
  cursor: pointer;
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
  background: #072e99;
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
</style>
