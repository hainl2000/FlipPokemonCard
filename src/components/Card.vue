<template>
  <div class="card" :class="{ disabled: isDisabled }" :style="{
      height: `${(1080 - 64) / Math.sqrt(cardContext.length) -16}px`,
      width: `${((1080 - 64) / Math.sqrt(cardContext.length) -16) *3/4}px`
    }">
    <div 
      class="card-inner" 
      :class="{'is-flipped' : isFlipped}" 
      @click="onToggleFlipCard"
    >
      <div class="card-face card-face-front">
        <div class="card-content">
        </div>
      </div>
      <div class="card-face card-face-back">
        <div class="card-content" :style="{ backgroundImage: `url(${require('@/assets/' + imgBackFaceUrl)})` }">
          
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="css" scoped>
  .card {
    display: inline-block;
    margin-right: 3rem;
    margin-bottom: 3rem;
  }

  .card-inner {
    width: 100%;
    height: 100%;
    transition: transform 1s;
    transform-style: preserve-3d;
    cursor: pointer;
    position: relative;
  }

  .card-inner.is-flipped {
    transform: rotateY(-180deg);
  }

  .card-face {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    overflow: hidden;
    border-radius: 1rem;
    padding: 1rem;
    box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
  }

  .card-face-front .card-content {
    background:  url("../assets/images/icon_back.png") no-repeat center center;
    background-size: 40px 40px;
    height: 100%;
    width: 100%;
  }

  .card-face-back {
    background-color: var(--light);
    transform: rotateY(-180deg);
  }

  .card-face-back .card-content {
    background-size: contain;
    background-position: center center;
    background-repeat: no-repeat;
    height: 100%;
    width: 100%;
  }

  .card-disabled .card-inner {
    cursor: default;
  }
  
</style>

<script>
export default {
  props: {
    card:{
      type: [String,Number,Array,Object],

    },
    imgBackFaceUrl: {
      type: String,
      required: true,
    },
    cardContext: {
      type: Array,
      default: function() {
        return []
      }
    }
  },
  data() {
    return {
      isDisabled: false,
      isFlipped: false
    };
  },
  methods: {
    onToggleFlipCard() {
      if(this.isDisabled) return false;
      this.isFlipped = !this.isFlipped;
      if(this.isFlipped) this.$emit("onFlip",this.card);
    },

    onFlipBackCard() {
      this.isFlipped = false;
    },

    onEnable() {
      this.isDisabled = true;
    }
  },
}
</script>
