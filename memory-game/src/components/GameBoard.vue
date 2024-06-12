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
        "./cardGifs/24.gif",
        "./cardGifs/25.gif",
        "./cardGifs/26.gif",
      ],
      flippedCards: [],
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
      const cardIndex = this.cards.findIndex((card) => card.id === id);
      if (this.flippedCards.length < 2 && !this.cards[cardIndex].flipped) {
        this.cards[cardIndex].flipped = true;
        this.flippedCards.push(this.cards[cardIndex]);

        if (this.flippedCards.length === 2) {
          this.checkForMatch();
        }
      }

      if (this.cards.every((card) => card.flipped)) {
        this.levelComplete = true;
        this.currentGif = this.gifs[this.level - 1];
      }
    },
    checkForMatch() {
      const [firstCard, secondCard] = this.flippedCards;
      if (firstCard.image === secondCard.image) {
        this.flippedCards = [];
      } else {
        setTimeout(() => {
          firstCard.flipped = false;
          secondCard.flipped = false;
          this.flippedCards = [];
        }, 1000);
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
  gap: 40px;
}
</style>
