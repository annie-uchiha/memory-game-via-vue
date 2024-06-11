<template>
    <div class="card" @click="flipCard" :class="{ flipped: isFlipped }">
      <div class="card-front">
      </div>
      <div class="card-back">
        <img :src="image" alt="Card Image" />
      </div>
    </div>
  </template>
  
  <script>
  export default {
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
    methods: {
      flipCard() {
        this.isFlipped = !this.isFlipped;
        this.$emit('card-flipped', this.id);
      },
    },
  };
  </script>
  
  <style scoped>
  .card {
    width: 100px;
    height: 150px;
    perspective: 1000px;
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
    background: #eee;
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
  