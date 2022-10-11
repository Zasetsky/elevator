<template>
  <div class="main">
    <Elevator :position="position" />
    <Floor 
      v-for="(item, id) in items" :key="id"
      @buttonClick="button"
      :id="id"/>
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
      items: [0,0,0,0,0],
      position: 0
    }
  },
  methods: {
    button(id) {
      this.position = id * 125
      console.log(this.position)
    },
  }
}

</script>

<style lang="scss">
  .main {
  }
</style>
