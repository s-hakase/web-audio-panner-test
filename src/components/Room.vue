<template>
  <div ref="room" class="room" @mousemove="mousemove" :style="roomStyle">
    <AudioNode
      ref="audioNodes"
      v-for="an in audioNodes"
      :key="an.id"
      :stream="an.stream"
      :position="position"
      :offsetX="x/2"
      :offsetY="y/2" />
    <div class="listener"></div>
  </div>
</template>

<script>
import AudioNode from './AudioNode.vue'

export default {
  components: {
    AudioNode
  },

  data() {
    return {
      position: { x: 0, y: 0 },
      audioNodes: []
    }
  },

  async mounted() {
    const stream = await navigator.mediaDevices.getUserMedia({ audio: true, video: false })
    this.audioNodes.push({
      id: 1,
      stream
    })
  },

  props: {
    x: {
      type: Number,
      required: true
    },

    y: {
      type: Number,
      required: true
    }
  },

  computed: {
    roomStyle() {
      return { width: `${this.x}px`, height: `${this.y}px` }
    },

    boundingClientRect() {
      return this.$refs["room"].getBoundingClientRect()
    }
  },

  methods: {
    play() {
      this.$refs["audioNodes"].forEach(node => {
        node.play()
      })
    },

    pause() {
      this.$refs["audioNodes"].forEach(node => {
        node.pause()
      })
    },

    mousemove(e) {
      const x = e.clientX - this.boundingClientRect.x,
            y = e.clientY - this.boundingClientRect.y
      const position = {x, y}
      this.position = position
    }
  }
}
</script>

<style scoped>
.room {
  position: relative;
  background: gray;
  overflow: hidden;
}

.listener {
  position: absolute;
  width: 20px;
  height: 20px;
  background: blue;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
</style>
