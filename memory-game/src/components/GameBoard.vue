<template>
  <div>
    <div class="grid">
      <Card
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
import Card from "./Card.vue";
import LevelComplete from "./LevelComplete.vue";
import _ from "lodash";

export default {
  components: { Card, LevelComplete },
  data() {
    return {
      cards: [],
      level: 1,
      levelComplete: false,
      currentGif: "",
      gifs: [
        "path/to/gif1.gif",
        "path/to/gif2.gif",
        "path/to/gif3.gif",
        "path/to/gif4.gif",
        "path/to/gif5.gif",
      ],
      cardImages: [
        "path/to/image1.jpg",
        "path/to/image2.jpg",
        "path/to/image3.jpg",
        "path/to/image4.jpg",
        "path/to/image5.jpg",
        "path/to/image6.jpg",
        "path/to/image7.jpg",
        "path/to/image8.jpg",
        "path/to/image9.jpg",
        "path/to/image10.jpg",
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
      const numCards = this.level * 4;
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
      if (flippedCards.length === 2) {
        if (flippedCards[0].image === flippedCards[1].image) {
        } else {
          setTimeout(() => {
            flippedCards.forEach((card) => (card.flipped = false));
          }, 1000);
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
