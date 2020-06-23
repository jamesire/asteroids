<template>
    <div class="dot" :style="'height: ' + h + 'px; width: ' + w + 'px;'" >
    </div>
</template>

<script>
export default {
  name: 'Asteroid',
  data: function() {
      return {
          h: 0,
          w: 0
      }
  },
  methods: {
      setDimensions: function(h, w) {
          this.h = h * 10;
          this.w = w * 10;
      }
  }
}
</script>

<style scoped>
.dot {
  height: 0;
  width: 0;
  background-color: white;
  border: solid 2px black;
  border-radius: 50%;
  display: inline-block;
}
</style>