<template>
  <div>
    <div :class="['grid', `grid-level-${level}`]">
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
        "dragon.gif",
        "niffler.gif",
        "fireworks.webp",
        "witch.gif",
        "snitch.gif",
      ],
      cardImages: [
        "1.gif",
        "2.gif",
        "3.gif",
        "4.gif",
        "5.gif",
        "6.gif",
        "7.gif",
        "8.gif",
        "9.gif",
        "10.gif",
        "11.gif",
        "12.gif",
        "13.gif",
        "14.gif",
        "15.gif",
        "16.gif",
        "17.gif",
        "18.gif",
        "19.gif",
        "20.gif",
        "21.gif",
        "22.gif",
        "23.gif",
        "24.gif",
        "25.gif",
        "26.gif",
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
        this.currentGif = require(`@/gifs/${this.gifs[this.level - 1]}`);
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
  grid-row-gap: 20px;
  grid-column-gap: 10px;
}

.grid-level-1 {
  grid-template-columns: repeat(5, 1fr);
  grid-template-rows: repeat(2, 1fr);
}

.grid-level-2 {
  grid-template-columns: repeat(5, 1fr);
  grid-template-rows: repeat(3, 1fr);
}

.grid-level-3 {
  grid-template-columns: repeat(5, 1fr);
  grid-template-rows: repeat(4, 1fr);
}

.grid-level-4 {
  grid-template-columns: repeat(6, 1fr);
  grid-template-rows: repeat(5, 1fr);
}

.grid-level-5 {
  grid-template-columns: repeat(7, 1fr);
  grid-template-rows: repeat(4, 1fr);
}

media (max-width: 768px) {
  .grid {
    gap: 10px;
  }
  .card {
    width: 160px;
    height: 120px;
  }
}

@media (max-width: 480px) {
  .grid {
    gap: 5px;
  }
  .card {
    width: 110px;
    height: 90px;
  }
}
</style>
