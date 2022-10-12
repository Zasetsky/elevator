<template>
  <div 
    class="caption-text"
    :class="{openDoors: isOpenDoors}" 
    :style="{marginTop: y + 'px', transition: speed + 's'}">
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
      isOpenDoors: {
        type: Boolean
      }
  },
  computed: {
    y() {
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
.openDoors {
    background-color: yellow;
  }
</style>