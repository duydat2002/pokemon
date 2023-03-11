<template>
  <div class="pd" :style="{ padding: mode < 8 ? '0 8px' : '0 3px' }">
    <div
      :class="['card', { disable: isDisabled, small: mode >= 8 }]"
      :style="{
        width: `${cardWidth}px`,
        height: `${cardHeight}px`,
      }"
      @click="onToggleCard()"
    >
      <div :class="['card__inner', { 'is-flipped': isFlipped }]">
        <div class="card__face card--front">
          <div
            class="card__content"
            :style="{
              backgroundImage: `url('${require('@/assets/' + imgCardURL)}')`,
            }"
          ></div>
        </div>
        <div class="card__face card--back">
          <div class="card__content"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    card: {
      type: Object,
    },
    checkCards: {
      type: Array,
    },
    mode: {
      type: Number,
    },
    imgCardURL: {
      type: String,
      required: true,
    },
    cardWidth: {
      type: Number,
    },
    cardHeight: {
      type: Number,
    },
  },
  data() {
    return {
      isFlipped: false,
      isDisabled: false,
    };
  },
  methods: {
    onToggleCard() {
      if (this.isDisabled) return;
      if (this.checkCards.length >= 2) return;
      this.isFlipped = !this.isFlipped;
      this.isDisabled = true;
      if (this.isFlipped) this.$emit("onFlip", this.card);
    },
    onFlipBackCard() {
      this.isFlipped = false;
      this.isDisabled = false;
    },
    onDisableCard() {
      this.isDisabled = true;
    },
  },
};
</script>

<style>
.card__inner {
  position: relative;
  width: 100%;
  height: 100%;
  transition: 1s;
  transform-style: preserve-3d;
  cursor: pointer;
}

.card.disable .card__inner {
  cursor: default;
}

.card__inner.is-flipped {
  transform: rotateY(180deg);
}

.card__face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  border-radius: 10px;
  padding: 10px;
  box-shadow: 0 3px 18px 3px rgba(0, 0, 0, 0.2);
}

.card.small .card__face {
  padding: 3px;
}

.card--front {
  background-color: var(--light);
  transform: rotateY(-180deg);
}

.card--front .card__content {
  background-position: center center;
  background-repeat: no-repeat;
  background-size: contain;
  height: 100%;
  width: 100%;
}

.card--back .card__content {
  background: url("../assets/images/icon_back.png") no-repeat center center;
  background-size: contain;
  height: 100%;
  width: 100%;
}
</style>
