<template>
<div class="screen">
  <div 
    class="screen-inner" 
    :style="{
      width: `${((((960 - 64) / Math.sqrt(cardContext.length) -16)*3) *3 / 4 + 16)*Math.sqrt(cardContext.length)}px`
      }"
  >
    <card-flip 
      v-for="(card, index) in cardContext" 
      :key="index" 
      :ref="`card-${index}`"
      :imgBackFaceUrl="`images/${card}.png`"
      :card="{ index, value: card }"
      :cardContext = "cardContext"
      @onFlip="checkRule($event)"
    />
  </div>
</div>
</template>

<script>

import CardFlip from "./Card";
  
export default {
  props: {
    cardContext: {
      type: Array,
      default: function() {
        return [];
      },
    },
  },
 components: {
  CardFlip
 },
 data() {
  return {
    rules: [],
  }
 },

 methods: {
  checkRule(card) {
    if(this.rules.length === 2) return false;
    this.rules.push(card);
    if(this.rules.length === 2 && this.rules[0].value === this.rules[1].value) {
      console.log("Right");

      this.$refs[`card-${this.rules[0].index}`][0].onEnable();
      this.$refs[`card-${this.rules[1].index}`][0].onEnable();
      this.rules = [];

      const disabledElements = document.querySelectorAll(".screen .card.disabled");
      console.log(disabledElements);
      if(disabledElements && disabledElements.length === this.cardContext.length -2)
      { 
        setTimeout( () => {
          this.$emit("onFinish")
        },1000);
      }
    }
    else if(this.rules.length === 2 && this.rules[0].value !== this.rules[1].value) {
      console.log("Wrong");
      setTimeout(() =>{
        console.log(this.$refs);
        this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
        this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();
        this.rules = [];
      },1000)
      
    }
    else return false;
  }
 } 
}

</script>

<style lang="css" scoped> 
  .screen {
    width: 100%;
    height: 100vh;
    position: absolute;
    top: 0;
    left: 0;
    z-index: 2;
    background-color: var(--dark);
    color: var(--light);
  }
  
  .screen-inner{
    display: flex;
    flex-wrap: wrap;
    margin: 2rem auto;
  }
</style>
