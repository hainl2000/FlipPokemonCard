<template>
  <div class="main-screen-container">
    <h1>POKEMON FLIP CARD</h1>
    <h3>Select mode</h3>
    <div class="actions">
      <button @click="onStart(16)">
        <span>4x4</span>
        <span>Easy</span>
      </button>
      <button @click="onStart(36)">
        <span>6x6</span>
        <span>Medium</span>
      </button>
      <button @click="onStart(64)">
        <span>8x8</span>
        <span>Hard</span>
      </button>
      <button @click="onStart(100)">
        <span>10x10</span>
        <span>Insane</span>
      </button>
    </div>
  </div>
</template>

<script>
export default {
    setup() {

    },
    methods: {
      onStart(totalOfCards) {
        this.$emit("onStart",{totalOfCards})
      },
    },  
}
</script>

<style>

</style>
