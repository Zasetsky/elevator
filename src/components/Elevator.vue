<template>
  <div class="shaft">
    <div 
      class="shaft__elevator"
      :class="{openDoors: isOpenDoors}" 
      :style="{marginTop: position + 'px', transition: speed + 's'}">
    </div>
    <Floor v-for="(floor, id) in floors" :key="id"/>
    <Button 
      v-for="(button, id) in floorsButton" :key="id"
      :floorsButton="floorsButton"
      :id="id"
      @buttonClick="addToQueue"
    />
  </div>
</template>
<script>
import Floor from './Floor.vue'
import Button from './Button.vue'

export default {
  name: "ElevatorComponent",
  components: { Floor, Button },
  data() {
      return {
          isActiveElevator: false,
          isOpenDoors: false,
          floorsCount: 10,
          position: 500,
          height: 125,
          oldPosition: 500,
          floorsButton: [],
          tempID: 4,
          queueArr: []
      };
  },
  computed: {
      speed() {
          return Math.abs(this.position - this.oldPosition) / this.height;
      },
  },
  watch: {
  },
  mounted() {
    for(let i = 0; i < this.floorsCount; i++) {
      this.floors.push({
        id: i,
        isActive: false
      })
    }
  },
  methods: {
    addToQueue(id) {
        if(this.tempID != id){
          this.queue(id);
        if(!this.isActiveElevator && this.queueArr.length > 0) {
          const x = this.queueArr.shift();
          this.move(x);
        }
      }
    },

    move(id) {
      this.tempID = id;
      this.isActiveElevator = true;
      this.oldPosition = this.position;  
      this.position = id * this.height;
      setTimeout(this.openDoors, this.speed * 1000);
    },

    openDoors() {
        this.isOpenDoors = true;
        setTimeout(this.closeDoors, 3000);
    },

    closeDoors() {
      this.isOpenDoors = false;
      this.isActiveElevator = false;
      this.floors[this.tempID].isActive = false;
      if(!this.isActiveElevator && this.queueArr.length > 0) {
        const x = this.queueArr.shift();
        this.move(x);
      }
    },

    queue(id) {
      if(!this.floors[id].isActive) {
        this.queueArr.push(id);
        this.floors[id].isActive = true;
        console.log(this.queueArr);
      }
    },
  }
}
</script>
<style lang="scss">
.shaft {
  display: grid;
  position: relative;
  grid-template-columns: 0.1fr 0.1fr;
}
.shaft__elevator {
    position: absolute;
    width: 100px;
    height: 100px;
    margin-left: 50px;
    background-color: red; 
  }
.openDoors {
  background-color: yellow;
  transition: all 1s !important;
}

</style>