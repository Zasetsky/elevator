<template>
  <div 
    class="caption-text" 
    :style="{marginTop:y + 'px', transition: speed + 's'}">
  </div>
</template>
<script>
export default {
  name: 'ElevatorComponent',
  props: {
      position: {
        type: Number,
      },
      speed: {
        type: Number
      },
    },
  computed: {
    y(){
      return this.position
    }
  }
}
</script>
<style lang="scss">
.caption-text {
    position: absolute;
    width: 100px;
    height: 100px;
    margin-left: 50px;
    background-color: red;

  }
</style>