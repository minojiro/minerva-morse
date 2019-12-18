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
const CHAR_OFFSET = 2 // 符号ごとの間隔
const DOT_AND_SPACE_WIDTH = 9 // `.` と ` ` の幅
const MORSE_HEAD_DEGREE = -135 // コミックス3巻によると左斜め上から時計回りに読むらしい

const degreeToRadian = (degree) => degree * Math.PI / 180

const getPathTagDAttrValue = (degree1, degree2) => {
  const radian1 = degreeToRadian(degree1 + CHAR_OFFSET)
  const radian2 = degreeToRadian(degree2 - CHAR_OFFSET)
  const x1 = Math.sin(radian1) * R + O
  const y1 = Math.cos(radian1) * R + O
  const x2 = Math.sin(radian2) * R + O
  const y2 = Math.cos(radian2) * R + O
  const f1 = (degree2 - degree1) >= 180 ? 1 : 0
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
    signPathes() {
      const morseString = morse.encode(this.message)
      const chars = `${morseString} `.split('')
      const barCount = chars.filter(sign => sign === '-').length
      const dotAndSpaceCount = chars.length - barCount
      const barWidth = (360 - DOT_AND_SPACE_WIDTH * dotAndSpaceCount) / barCount
      let previousEndDegree = MORSE_HEAD_DEGREE

      return chars.map(char => {
        const charWidth = char === '-' ? barWidth : DOT_AND_SPACE_WIDTH
        const startDegree = previousEndDegree
        const endDegree = startDegree + charWidth
        previousEndDegree = endDegree
        if (char === ' ') {
          return ''
        }
        // pathタグのd属性の値を取得
        return getPathTagDAttrValue(startDegree, endDegree)
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
