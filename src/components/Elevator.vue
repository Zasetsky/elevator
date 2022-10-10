<template>
    <div class="caption-text" :style="{marginTop:y + 'px'}" @click="click()">
        
    </div>
</template>
<script>
export default {
    name: 'ElevatorComponent',
    data() {
    return {
      y: 500
    }
  },
    methods: {
    click() {
      this.y -=99
      if(this.y < 0) {
        this.y += 500
      }
    }
  },
}
</script>
<style lang="scss">
.caption-text {
    width: 100px;
    height: 100px;
    margin-left: 50px;
    background-color: red;
    transition: all 3s;
  }
</style>