<template>
    <div class="dot" :style="'height: ' + h + 'px; width: ' + w + 'px; background-size: ' + w + 'px ' + h + 'px;'">
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

//:style="'height: ' + h + 'px; width: ' + w + 'px; background-size: ' + (h * w) + 'px;'" 
</script>

<style scoped>
.dot {
    content: "";
    position: absolute;
    top: 50%;
    left: 60%;
    background-image: url('../resources/octagon.svg');
    background-position: center;
    background-repeat: no-repeat;
    animation:spin 4s linear infinite;
}

@keyframes spin{
	from{transform:rotate(0deg)}
	to{transform:rotate(360deg)}	
}
</style>