<template>
  <div class="screen">
    <h1>congratulations</h1>
    <p>{{Math.round(time/1000)}} seconds</p>
    <button @click="onStartAgain">Play again</button>
  </div>
</template>

<script>
export default {
  props: {
    time: {
      Type: Number,
      required: true
    },
  },
  methods: {
    onStartAgain() {
      this.$emit("onStartAgain")
    }
  }
}
</script>
