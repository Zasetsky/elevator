<template>
  <div class="main">
    <Elevator 
      :position="position" 
      :speed="speed"
    />
    <Floor 
      v-for="(item, id) in items" :key="id"
      @buttonClick="button"
      :id="id"
    />
  </div>
</template>

<script>
import Elevator from "./components/Elevator.vue";
import Floor from "./components/Floor.vue";

export default {
  name: 'App',
  components: {
    Elevator,
    Floor
  },
  data() {
    return {
      items: 5,
      position: 500,
      height: 125,
      oldPosition: 0
    }
  },
  computed: {
    speed() {
      return Math.abs(this.position - this.oldPosition) / this.height
    }
  },
  methods: {
    button(id) {
      this.oldPosition = this.position
      if(!this.isActive) {
        this.isActive = true
        this.position = id * this.height
        setTimeout(this.busyElevator, this.speed * 1000)
        console.log(this.speed)
      }
    },
    busyElevator() {
      this.isActive = false
    }
  }
}

</script>

<style lang="scss">
  .main {
  }
</style>
