<template>
    <div class="ellipse" :style="'height: ' + magnifiedH + 'px; width: ' + magnifiedW + 'px;  background-size: ' + magnifiedH + 'px ' + magnifiedW + 'px;'">
    </div>
</template>

<script>
export default {
  name: 'Asteroid',
  data: function() {
      return {
          height: 0,
          width: 0,
          magnifiedH: 0,
          magnifiedW: 0
      }
  },
  methods: {
      setDimensions: function(h, w) {
          this.height = h * 2;
          this.width = w * 2;
          this.magnifiedH = this.height;
          this.magnifiedW = this.width;
      },
      applyMagnification: function(multiplier) {
          this.magnifiedH = this.height * multiplier;
          this.magnifiedW = this.width * multiplier;
      }
  }
}

</script>

<style scoped>
.ellipse {
    content: "";
    position: absolute;
    left: 60%;
    top: 20%;
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