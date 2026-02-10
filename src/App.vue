<script setup lang="ts">
import { computed, ref } from 'vue'

const clickedYes = ref(false)

const correctButtonFontSize = ref(16)
const correctButtonBgColor = ref('')
const correctButtonColor = ref('')
const correctButtonRef = ref<HTMLElement | null>(null)

const wrongButtonFontSize = ref(16)
const wrongButtonTopPosition = ref(window.innerHeight / 2)
const wrongButtonLeftPosition = ref(window.innerWidth / 2 + 50)

const hoverCount = ref(0)
const isSerieuxHidden = ref(true)
const isNonHidden = ref(false)

const correctButtonStyle = computed(() => `font-size: ${correctButtonFontSize.value}px; border-color: ${correctButtonBgColor.value}; background-color: ${correctButtonBgColor.value}; color: ${correctButtonColor.value};`)

const wrongButtonStyle = computed(() => {
  if (hoverCount.value === 0) {
    return `font-size: ${wrongButtonFontSize.value}px;`
  }
  return `font-size: ${wrongButtonFontSize.value}px; top: ${wrongButtonTopPosition.value}px; left: ${wrongButtonLeftPosition.value}px; position: absolute;`
})

function isOverlapping(x: number, y: number, buttonWidth: number, buttonHeight: number): boolean {
  if (!correctButtonRef.value) return false

  const correctRect = correctButtonRef.value.getBoundingClientRect()
  const padding = 20 // Extra spacing to avoid being too close

  // Check if rectangles overlap
  return !(
    x + buttonWidth + padding < correctRect.left ||
    x - padding > correctRect.right ||
    y + buttonHeight + padding < correctRect.top ||
    y - padding > correctRect.bottom
  )
}

function getRandomPosition(): { top: number; left: number } {
  const buttonWidth = 120
  const buttonHeight = 50
  const maxAttempts = 50
  let attempts = 0

  while (attempts < maxAttempts) {
    const top = Math.floor(Math.random() * (window.innerHeight - buttonHeight))
    const left = Math.floor(Math.random() * (window.innerWidth - buttonWidth))

    if (!isOverlapping(left, top, buttonWidth, buttonHeight)) {
      return { top, left }
    }
    attempts++
  }

  // Fallback: just return a random position if we can't find a non-overlapping one
  return {
    top: Math.floor(Math.random() * (window.innerHeight - buttonHeight)),
    left: Math.floor(Math.random() * (window.innerWidth - buttonWidth))
  }
}

function hoveredWrongButton() {
  hoverCount.value++

  const { top, left } = getRandomPosition()
  wrongButtonTopPosition.value = top
  wrongButtonLeftPosition.value = left

  if (hoverCount.value > 3 && hoverCount.value <= 6) {
    correctButtonFontSize.value += 12
  }
  if (hoverCount.value === 7) {
    isSerieuxHidden.value = false
  }
  if (hoverCount.value > 7 && hoverCount.value < 10) {
    wrongButtonFontSize.value -= 2
  }
  if (hoverCount.value === 10) {
    isSerieuxHidden.value = true
    correctButtonFontSize.value = 200
    isNonHidden.value = true
  }

}
</script>

<template>
  <div class="wrapper">
    <div v-if="!clickedYes" class="question-container">
      <div class="div-valentine">Est ce que, est ce que tu veux être ma valentine ?</div>
      <div style="font-size: 18px">stp</div>
      <img
        src="/cameron.webp"
        alt="Cameron"
        class="valentine-image"
      />
      <div class="div-serieux" v-if="!isSerieuxHidden">Sérieux ??</div>
      <div class="buttons-container">
        <button ref="correctButtonRef" class="button-base button-correct" :style="correctButtonStyle" @click="clickedYes = true">
          Oui
        </button>
        <button v-if="!isNonHidden" class="button-wrong" :style="wrongButtonStyle" @mouseover="hoveredWrongButton" @click="hoveredWrongButton">
          Non
        </button>
      </div>
    </div>
    <div v-else class="answer-container">
      <div class="div-oui">
        Owii je t'aime <span style="color: red;">❤️</span>
      </div>
      <img
        src="/otters.jpg"
        class="yes-image"
    </div>
  </div>

</template>

<style scoped>

.wrapper {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  height: 100dvh; /* Dynamic viewport height for mobile */
  background: linear-gradient(135deg, #ffeef8 0%, #fff9e6 50%, #ffe6f0 100%);
  color: #d63384;
  overflow: hidden;
}

.question-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 30px;
  height: 100vh;
  height: 100dvh; /* Dynamic viewport height for mobile */
  padding: 20px;
  overflow: hidden;
}

.answer-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 30px;
  height: 100vh;
  height: 100dvh; /* Dynamic viewport height for mobile */
  padding: 20px;
  overflow: hidden;
}

.div-valentine {
  font-size: 30px;
  font-weight: 700;
  text-shadow: 2px 2px 4px rgba(214, 51, 132, 0.1);
  text-align: center;
}

.valentine-image {
  max-width: 200px;
  border-radius: 16px;
  border: 4px solid #ffc0cb;
  box-shadow: 0 8px 20px rgba(255, 192, 203, 0.4);
}

.buttons-container {
  display: flex;
  gap: 20px;
}

.yes-image {
  max-width: 300px;
  border-radius: 16px;
  border: 4px solid #ffc0cb;
  box-shadow: 0 8px 20px rgba(255, 192, 203, 0.4);
}

.div-serieux {
  font-size: 30px;
  color: #d63384;
  font-weight: 700;
  text-shadow: 2px 2px 4px rgba(214, 51, 132, 0.2);
  text-align: center;
}

.div-oui {
  font-size: 30px;
  font-weight: 700;
  text-shadow: 2px 2px 4px rgba(214, 51, 132, 0.1);
  text-align: center;
}

@media (max-width: 768px) {
  .button-base {
    padding: 10px 30px;
  }

  .div-valentine {
    font-size: 24px;
    padding: 0 20px;
    text-align: center;
  }

  .valentine-image {
    max-width: 150px;
  }
}

.button-base {
  min-width: 120px;
  padding: 12px 40px;
  cursor: pointer;
  border-radius: 25px;
  font-weight: 600;
  transition: all 0.3s ease;
  letter-spacing: 0.5px;

  &.button-correct {
    border: 2px solid #ffd700;
    background: linear-gradient(135deg, #fff9e6 0%, #fffaed 100%);
    color: #b8860b;
    box-shadow: 0 4px 15px rgba(255, 215, 0, 0.3);

    &:hover {
      transform: translateY(-2px);
      box-shadow: 0 6px 20px rgba(255, 215, 0, 0.5);
      background: linear-gradient(135deg, #fff5d6 0%, #fff8e1 100%);
      border-color: #ffcc00;
    }

    &:active {
      transform: translateY(0);
    }
  }
}

.button-wrong {
  min-width: 120px;
  padding: 12px 40px;
  cursor: pointer;
  border-radius: 25px;
  font-weight: 600;
  transition: all 0.3s ease;
  letter-spacing: 0.5px;
  border: 2px solid #ff69b4;
  background: linear-gradient(135deg, #ffc0cb 0%, #ffb6c1 100%);
  color: #c71585;
  box-shadow: 0 4px 15px rgba(255, 105, 180, 0.4);

}
</style>
