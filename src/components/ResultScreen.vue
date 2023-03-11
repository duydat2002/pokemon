<template>
  <h1>✨ Congratulations ✨</h1>
  <h3>{{ Math.round(timer / 920) }} seconds</h3>
  <button @click="this.$emit('startAgain')">Start Again</button>
</template>

<script>
export default {
  props: {
    timer: {
      type: Number,
    },
  },
};
</script>

<style scoped>
h1 {
  font-size: 80px;
}

h3 {
  margin-top: 24px;
  font-size: 48px;
}

button {
  font: var(--font);
  background: transparent;
  box-shadow: none;
  border: 1px solid var(--light);
  color: var(--light);
  margin: 16px;
  padding: 16px 20px;
  border-radius: 8px;
  font-size: 20px;
  cursor: pointer;
  transition: 0.3s ease-in-out;
}

button:hover {
  background-color: var(--light);
  color: var(--dark);
}
</style>
