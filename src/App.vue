<template>
  <div id="app">
    <h1 class="heading">ミネルヴァさんのモールススタンプ</h1>
    <input
      class="message-input"
      :placeholder="defaultMessage"
      type="text"
      v-model="message" />
    <p class="hankaku">メッセージを半角英数字でどうぞ</p>
    <div class="morse-stamp-wrap">
      <MorseStamp :message="message || defaultMessage" />
    </div>
  </div>
</template>

<script>
import MorseStamp from './components/MorseStamp.vue'

export default {
  name: 'app',
  data() {
    return {
      message: '',
      defaultMessage: 'SOS',
    }
  },
  components: {
    MorseStamp,
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  max-width: 400px;
  margin: 50px auto;
  padding: 0 20px;
}

.heading {
  font-size: 16px;
  margin-bottom: 20px;
  color: #5e4f43;
}

.message-input {
  font-size: 20px;
  text-align: center;
  padding: 0.5em;
  margin-bottom: 5px;
  border-radius: 5px;
  width: 100%;
  box-sizing: border-box;
  border: 2px solid #ccc;
}

.hankaku {
  font-size: 12px;
  margin: 0 0 35px;
}
</style>
