<template>
  <div class="play-screen">
    <div class="back-button" @click="this.$emit('backToMain')">
      <i class="fa-solid fa-arrow-left"></i>
    </div>
    <div
      class="cards__container"
      ref="cardContainer"
      :style="{
        width: `${cardContainerWidth}px`,
      }"
    >
      <card
        v-for="(card, i) in cardArray"
        :key="i"
        :ref="`card-${i}`"
        :card="{ card, index: i }"
        :imgCardURL="`images/${card}.png`"
        :cardWidth="cardWidth"
        :cardHeight="cardHeight"
        :checkCards="checkCards"
        :mode="mode"
        @onFlip="checkTwoCard($event)"
      ></card>
    </div>
  </div>
</template>

<script>
import Card from "./Card.vue";

export default {
  props: {
    mode: {
      type: Number,
    },
    cardArray: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      isMounted: false,
      checkCards: [],
      cardGap: 16,
    };
  },
  methods: {
    checkTwoCard(card) {
      if (this.checkCards.length == 1 && card.index == this.checkCards[0].index)
        return;
      this.checkCards.push(card);
      if (this.checkCards.length == 2) {
        if (this.checkCards[0].card == this.checkCards[1].card) {
          this.$refs[`card-${this.checkCards[0].index}`][0].onDisableCard();
          this.$refs[`card-${this.checkCards[1].index}`][0].onDisableCard();
          this.checkCards = [];

          setTimeout(() => {
            if (
              document.querySelectorAll(".cards__container .card.disable")
                .length == this.cardArray.length
            ) {
              this.$emit("onFinish");
            }
          }, 800);
        } else {
          setTimeout(() => {
            this.$refs[`card-${this.checkCards[0].index}`][0].onFlipBackCard();
            this.$refs[`card-${this.checkCards[1].index}`][0].onFlipBackCard();
            this.checkCards = [];
          }, 800);
        }
      }
    },
  },
  computed: {
    cardHeight() {
      if (!this.isMounted) return;
      return (
        (this.$refs.cardContainer.clientHeight - this.cardGap * this.mode) /
        this.mode
      );
    },
    cardWidth() {
      return (this.cardHeight * 3) / 4;
    },
    cardContainerWidth() {
      return (this.cardWidth + this.cardGap) * this.mode;
    },
  },
  components: { Card },
  mounted() {
    this.isMounted = true;
    this.cardGap = this.mode < 8 ? 16 : 6;
  },
};
</script>

<style scoped>
.play-screen {
  position: relative;
  width: 100%;
  min-width: 1000px;
  height: 100%;
  padding: 0 150px;
}

.back-button {
  position: absolute;
  top: 50px;
  right: 50px;
  width: 30px;
  height: 30px;
  display: flex;
  justify-content: center;
  align-items: center;
  color: var(--light);
  border: 1px solid var(--light);
  border-radius: 50%;
  cursor: pointer;
  transition: 0.3s ease-in-out;
}

.back-button:hover {
  background: var(--light);
  color: var(--dark);
}

.cards__container {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
  height: 100%;
  color: var(--light);
  margin: 0 auto;
}
</style>
