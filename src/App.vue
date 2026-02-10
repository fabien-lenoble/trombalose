<script setup lang="ts">
import { computed, ref } from 'vue'

const correctButtonFontSize = ref(20)
const wrongButtonFontSize = ref(20)
const topPosition = ref(window.innerHeight / 2)
const leftPosition = ref(window.innerWidth / 2 + 100) // 50 is the distance from the correct button
const hoverCount = ref(0)
const isSerieuxHidden = ref(true)

const correctButtonStyle = computed(() => `font-size: ${correctButtonFontSize.value}px;`)

const wrongButtonStyle = computed(() => `font-size: ${wrongButtonFontSize.value}px; top: ${topPosition.value}px; left: ${leftPosition.value}px; position: absolute;`)

function hoveredWrongButton() {
  hoverCount.value++
  topPosition.value = Math.floor(Math.random() * (window.innerHeight - 50)) // 50 is the button height
  leftPosition.value = Math.floor(Math.random() * (window.innerWidth - 100)) // 100 is the button width

  if (hoverCount.value > 3 && hoverCount.value <= 6) {
    correctButtonFontSize.value += 10
  } else if (hoverCount.value > 6) {
    isSerieuxHidden.value = false
  }
}
</script>

<template>
  <div class="wrapper">
    <div class="div-valentine">Est ce que, est ce que tu veux être ma valentine ?</div>
    <img
      src="/cameron.webp"
      alt="Cameron"
      class="valentine-image"
      crossorigin="anonymous"
    />
    <button class="button-correct" :style="correctButtonStyle">
      Oui
    </button>
    <button class="button-wrong" :style="wrongButtonStyle" @mouseover="hoveredWrongButton">
      Non
    </button>
    <div class="div-serieux" v-if="!isSerieuxHidden">Sérieux ?</div>
  </div>
</template>

<style scoped>
.wrapper {
  position: relative;
  width: 100%;
  height: 100vh;
}
.div-valentine {
  position: absolute;
  top: calc(20%);
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 30px;
}
.valentine-image {
  position: absolute;
  top: calc(50%);
  left: 50%;
  transform: translate(-50%, -50%);
  max-width: 250px;
  border-radius: 8px;
}
.div-serieux {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 30px;
  color: red;
}
.button-correct {
  top: 50%;
  left: calc(50% - 150px);
  position: absolute
}
.button-wrong {
}
</style>
