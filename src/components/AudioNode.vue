<template>
  <div class="audioNode" :style="nodeStyle"></div>
</template>

<script>
export default {
  data() {
    return {
      audioctx: null,
      panner: null,
      source: null
    }
  },

  props: {
    stream: {
      type: MediaStream,
      required: true
    },

    position: {
      type: Object,
      required: true
    },

    offsetX: {
      type: Number,
      default: 0
    },

    offsetY: {
      type: Number,
      default: 0
    }
  },

  computed: {
    audioEl() {
      return this.$refs["audio"]
    },

    nodeStyle() {
      return {
        left: `${this.position.x}px`,
        top: `${this.position.y}px`
      }
    },

    audioPosition() {
      return {x: this.position.x - this.offsetX, y: 0, z: this.position.y - this.offsetY }
    }
  },

  mounted() {
    this.audioctx = new AudioContext()
    this.source = this.audioctx.createMediaStreamSource(this.stream)

    this.panner = this.audioctx.createPanner()
    this.panner.panningModel = 'HRTF'

    this.source.connect(this.panner)
    this.panner.connect(this.audioctx.destination)

    this.audioctx.suspend()
  },

  methods: {
    play() {
      console.info('play')
      this.audioctx.resume()
    },
    pause() {
      console.info('pause')
      this.audioctx.suspend()
    }
  },

  watch: {
    audioPosition(newValue) {
      const {x, y, z} = newValue
      const coef = 0.01
      console.info(x*coef, y*coef, z*coef)
      this.panner.setPosition(x*coef, y*coef, z*coef)
      // this.audioctx.listener.setPosition(x, y, z)
    }
  }
}
</script>

<style scoped>
.audioNode {
  position: absolute;
  border-radius: 50%;
  background: red;
  width: 20px;
  height: 20px;
  transform: translate(-50%, -50%);
}
</style>