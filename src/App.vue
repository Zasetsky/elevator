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
      id: 0,
      oldPosition: 0
    }
  },
  computed: {
    speed() {
      if(this.position == this.oldPosition - 125 || this.position == this.oldPosition + 125) {
        return 1
      } if(this.position == this.oldPosition - 250 || this.position == this.oldPosition + 250 ) {
        return 2
      } if(this.position == this.oldPosition - 375 || this.position == this.oldPosition + 375) {
        return 3
      } else {
        return 4
      }
    }
  },
  methods: {
    button(id) {
      this.oldPosition = this.position
      this.position = id * 125
      this.id = id
      console.log(this.speed);
    },
  }
}

</script>

<style lang="scss">
  .main {
  }
</style>
