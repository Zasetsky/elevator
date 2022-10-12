<template>
  <div class="main">
    <Elevator 
      :position="position" 
      :speed="speed"
      :isOpenDoors="isOpenDoors"
    />
    <Floor 
      v-for="(item, id) in items" :key="id"
      @buttonClick="move"
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
      oldPosition: 0,
      isActiveElevator: false,
      isOpenDoors: false
    }
  },
  computed: {
    speed() {
      return Math.abs(this.position - this.oldPosition) / this.height
    }
  },
  methods: {
    move(id) {
      this.oldPosition = this.position
      if(!this.isActiveElevator && !this.isOpenDoors) {
        this.isActiveElevator = true
        this.position = id * this.height

        setTimeout(this.deactivatingElevator, this.speed * 1000)
        setTimeout(this.openDoors, this.speed * 1000)
      }
    },

    deactivatingElevator() {
      this.isActiveElevator = false
    },

    openDoors() {
      this.isOpenDoors = true
      setTimeout(this.closeDoors, 3000)
    },

    closeDoors() {
      this.isOpenDoors = false
    }
  }
}

</script>

<style lang="scss">
  .main {
  }
</style>
