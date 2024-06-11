<template>
  <div>
    <div class="grid">
      <MemoryCard
        v-for="card in cards"
        :key="card.id"
        :image="card.image"
        :id="card.id"
        :flipped="card.flipped"
        @card-flipped="handleCardFlip"
      />
    </div>
    <LevelComplete
      v-if="levelComplete"
      :gif="currentGif"
      @next-level="nextLevel"
    />
  </div>
</template>

<script>
import MemoryCard from "./MemoryCard.vue";
import LevelComplete from "./LevelComplete.vue";
import _ from "lodash";

export default {
  components: { MemoryCard, LevelComplete },
  data() {
    return {
      cards: [],
      level: 1,
      levelComplete: false,
      currentGif: "",
      gifs: [
        "./gifs/dragon.gif",
        "./gifs/niffler.gif",
        "./gifs/fireworks.webp",
        "./gifs/witch.gif",
        "./gifs/snitch.gif",
      ],
      cardImages: [
        "./cardGifs/1.gif",
        "./cardGifs/2.gif",
        "./cardGifs/3.gif",
        "./cardGifs/4.gif",
        "./cardGifs/5.gif",
        "./cardGifs/6.gif",
        "./cardGifs/7.gif",
        "./cardGifs/8.gif",
        "./cardGifs/9.gif",
        "./cardGifs/10.gif",
        "./cardGifs/11.gif",
        "./cardGifs/12.gif",
        "./cardGifs/13.gif",
        "./cardGifs/14.gif",
        "./cardGifs/15.gif",
        "./cardGifs/16.gif",
        "./cardGifs/17.gif",
        "./cardGifs/18.gif",
        "./cardGifs/19.gif",
        "./cardGifs/20.gif",
        "./cardGifs/21.gif",
        "./cardGifs/22.gif",
        "./cardGifs/23.gif",
      ],
    };
  },
  created() {
    this.initializeGame();
  },
  methods: {
    initializeGame() {
      this.cards = [];
      this.generateCards();
    },
    generateCards() {
      const numCards = this.level * 4 + 6;
      const selectedImages = _.shuffle(this.cardImages).slice(0, numCards / 2);
      const pairedImages = _.shuffle([...selectedImages, ...selectedImages]);
      this.cards = pairedImages.map((image, index) => ({
        id: index,
        image,
        flipped: false,
      }));
    },
    handleCardFlip(id) {
      const flippedCards = this.cards.filter((card) => card.flipped);
      if (flippedCards.length < 2) {
        const cardIndex = this.cards.findIndex((card) => card.id === id);
        this.cards[cardIndex].flipped = true;

        if (flippedCards.length === 1) {
          const firstCard = flippedCards[0];
          const secondCard = this.cards[cardIndex];

          if (firstCard.image === secondCard.image) {
            // Cards match
          } else {
            // Cards do not match, flip back after delay
            setTimeout(() => {
              firstCard.flipped = false;
              secondCard.flipped = false;
            }, 1000);
          }
        }
      }

      if (this.cards.every((card) => card.flipped)) {
        this.levelComplete = true;
        this.currentGif = this.gifs[this.level - 1];
      }
    },
    nextLevel() {
      if (this.level < 5) {
        this.level++;
        this.levelComplete = false;
        this.generateCards();
      }
    },
  },
};
</script>

<style scoped>
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
  gap: 10px;
}
</style>
