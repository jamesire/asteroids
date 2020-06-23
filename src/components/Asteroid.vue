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
          this.h = h * 2;
          this.w = w * 2;
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
    box-shadow: 0px 0px 30px white;
    height: 25px;
    width: 25px;
    background-color: #bbb;
    border-radius: 50%;
    display: inline-block;
    animation:spin 4s linear infinite;
}

@keyframes spin{
	from{transform:rotate(0deg)}
	to{transform:rotate(360deg)}	
}
</style>