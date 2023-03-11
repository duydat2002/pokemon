<template>
  <div class="screen">
    <main-screen
      v-if="statusMatch == 'default'"
      @onBeforStart="onHandleBeforStart($event)"
    ></main-screen>
    <play-screen
      v-if="statusMatch == 'playing'"
      :cardArray="settings.cardArray"
      :mode="settings.mode"
      @backToMain="this.statusMatch = 'default'"
      @onFinish="onFinish()"
    ></play-screen>
    <result-screen
      v-if="statusMatch == 'finish'"
      :timer="timer"
      @startAgain="this.statusMatch = 'default'"
    ></result-screen>
  </div>
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import PlayScreen from "./components/PlayScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import { shuffled } from "./utils/shuffled";
export default {
  components: { MainScreen, PlayScreen, ResultScreen },
  name: "App",
  data() {
    return {
      totalOfCardSource: 64,
      statusMatch: "default",
      settings: {
        mode: 4,
        numberOfCard: 0,
        cardArray: [],
        startedAt: null,
      },
      timer: 0,
    };
  },
  methods: {
    onHandleBeforStart(mode) {
      this.settings.mode = mode;
      this.settings.numberOfCard = mode * mode;

      let arr = Array.from({ length: this.totalOfCardSource }, (_, i) => i + 1);
      arr = shuffled(arr).slice(-(this.settings.numberOfCard / 2));
      arr = arr.concat(arr);
      arr = shuffled(shuffled(shuffled(arr)));
      this.settings.cardArray = arr;

      this.timer = 0;
      this.settings.startedAt = new Date().getTime();
      this.statusMatch = "playing";
    },
    onFinish() {
      this.statusMatch = "finish";
      this.timer = new Date().getTime() - this.settings.startedAt;
    },
  },
};
</script>

<style scoped>
.screen {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 100%;
  height: 100vh;
  color: var(--light);
  background-color: var(--dark);
  overflow: hidden;
}
</style>
