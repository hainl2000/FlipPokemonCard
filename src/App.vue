<template>
  <main-screen v-if="statusMatch === 'default'" @onStart="onHandleBeforeStarting($event)"/>
  <interact-screen 
    v-if="statusMatch === 'onMatch'" 
    :cardContext="this.settings.cardContext" 
    @onFinish="onGetResult"
  />
  <footer-screen />
  
  <result-screen v-if="statusMatch === 'result'" :time="time" @onStartAgain="statusMatch = 'default'"/>
</template>

<script>
import MainScreen from "./components/MainScreen.vue"
import FooterScreen from "./components/FooterScreen.vue"
import InteractScreen from './components/InteractScreen.vue'

import { shuffle } from './utils/array'
import ResultScreen from './components/ResultScreen.vue'

export default {
  name: 'App',
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardContext: [],
        startedAt : null
      },
      statusMatch: "default",
      time : 0
    }
  },
  components: {
    MainScreen,
    FooterScreen,
    InteractScreen,
    ResultScreen
  },
  methods: {
    onHandleBeforeStarting(config) {
      console.log("running handle before starting",config);
      this.settings.totalOfCards = config.totalOfCards;
      const firstCard = Array.from(
        {length: this.settings.totalOfCards / 2}, 
        (_,i) => i+1
      );

      const secondCard = [...firstCard];

      const cards = [...firstCard, ...secondCard];

      this.settings.cardContext = shuffle(shuffle(cards));

      this.settings.startedAt = new Date().getTime();
      this.statusMatch = 'onMatch';
    },
    onGetResult() {
      this.time = new Date().getTime() - 1000 - this.settings.startedAt;
      this.statusMatch = "result";
    }
  },
}
</script>

<style>
</style>
