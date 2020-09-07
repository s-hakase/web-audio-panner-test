<template>
  <div id="app">
    <Controls @play="play" @pause="pause" />
    <Room :x="500" :y="500" ref="room" />
  </div>
</template>

<script>
import Room from './components/Room.vue'
import Controls from './components/Controls.vue'

export default {
  components: {
    Room,
    Controls
  },

  methods: {
    play() {
      this.$refs["room"].play()
    },
    pause() {
      this.$refs["room"].pause()
    }
  }
}
</script>

<style>
</style>
