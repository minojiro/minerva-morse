<template>
  <svg
    :height="svgWidth"
    :viewBox="`0 0 ${svgWidth} ${svgWidth}`"
    :width="svgWidth"
    class="morse-stamp"
    xmlns="http://www.w3.org/2000/svg"
  >
    <rect
      :height="svgWidth"
      :width="svgWidth"
      fill="#f6eace"
    />
    <path
      :d="signPath"
      :key="signPath"
      fill="none"
      stroke-linecap="round"
      stroke-width="3"
      stroke="#5e4f43"
      v-for="signPath in signPathes"
    />
    <image
      :height="imageWidth"
      :width="imageWidth"
      :x="imageX"
      :y="imageX"
      xlink:href="@/assets/animal_fukurou.png"
    />
  </svg>
</template>

<script>
import morse from 'morse'

const O = 100 // 中央
const R = 90 // モールスの円の半径
const MORSE_STARTS_DEG = -135 // コミックス3巻によると左斜め上から時計回りに読むらしい

const DOT = 1
const BAR = 2
const SPACE = 3

const degToRad = (deg) => deg * Math.PI / 180

const getSignPathDValue = (deg1, deg2) => {
  const rad1 = degToRad(deg1 + 2)
  const rad2 = degToRad(deg2 - 2)
  const x1 = Math.sin(rad1) * R + O
  const y1 = Math.cos(rad1) * R + O
  const x2 = Math.sin(rad2) * R + O
  const y2 = Math.cos(rad2) * R + O
  const f1 = (deg2 - deg1) >= 180 ? 1 : 0
  return `M ${y1},${x1} A ${R} ${R} 0 ${f1} 1 ${y2},${x2}`
}

export default {
  name: 'MorseStamp',
  props: {
    message: String,
  },
  data() {
    return {
      imageWidth: O,
      imageX: O / 2,
      svgWidth: O * 2,
    }
  },
  computed: {
    morse() {
      return morse.encode(this.message)
    },
    signPathes() {
      const morseString = morse.encode(this.message)
      const signs = `${morseString} `.split('').map(s => {
        if (s === '.') return DOT
        if (s === '-') return BAR
        return SPACE
      })
      const barCount = signs.filter(sign => sign === BAR).length
      const dotAndSpaceCount = signs.length - barCount
      const dotAndSpaceWidth = 9
      const barWidth = (360 - dotAndSpaceWidth * dotAndSpaceCount) / barCount
      let prevStartDeg = MORSE_STARTS_DEG

      return signs.map(sign => {
        const signWidth = sign === BAR ? barWidth : dotAndSpaceWidth
        const startDeg = prevStartDeg
        const endDeg = startDeg + signWidth
        prevStartDeg = endDeg
        if (sign === SPACE) {
          return ''
        }
        return getSignPathDValue(startDeg, endDeg)
      }).filter(s => s !== '')
    },
  },
}
</script>

<style scoped lang="scss">
.morse-stamp {
  width: 100%;
  height: 100%;
}
</style>
