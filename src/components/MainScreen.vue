<template>
  <h1>POKE MEMORIES</h1>
  <h4>Select mode to start game</h4>
  <div class="modes">
    <div class="mode" @click="selectMode(4)">
      <p class="mode__number">4x4</p>
      <p class="mode__text">Easy</p>
    </div>
    <div class="mode" @click="selectMode(6)">
      <p class="mode__number">6x6</p>
      <p class="mode__text">Normal</p>
    </div>
    <div class="mode" @click="selectMode(8)">
      <p class="mode__number">8x8</p>
      <p class="mode__text">Hard</p>
    </div>
    <div class="mode" @click="selectMode(10)">
      <p class="mode__number">10x10</p>
      <p class="mode__text">SuperHard</p>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    selectMode(mode) {
      this.$emit("onBeforStart", mode);
    },
  },
};
</script>

<style scoped>
h1 {
  font-size: 72px;
  text-transform: uppercase;
  color: var(--light);
}

h4 {
  font-size: 32px;
  color: var(--light);
}

.modes {
  display: flex;
  justify-content: space-between;
  margin-top: 32px;
}

.mode {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 150px;
  height: 150px;
  margin: 0 15px;
  border: 1px solid var(--light);
  border-radius: 20px;
  color: var(--light);
  cursor: pointer;
  transition: 0.3s ease-in-out;
}

.mode:hover {
  background: var(--light);
  color: var(--dark);
}

.mode__number {
  font-size: 32px;
}

.mode__text {
  margin-top: 8px;
  font-size: 20px;
}
</style>
