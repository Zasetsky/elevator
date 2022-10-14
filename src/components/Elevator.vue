<template>
  <div class="house">
    <div class="shaft">
      <div 
        v-for="el in elevator" :key="el.id"
        class="elevator"
        :class="{openDoors: el.isOpenDoors, 'newElevator' : el.id >= 1, 'newElevator2' : el.id >= 2 }" 
        :style="{marginTop: el.position + 'px', transition: speed + 's'}">
      </div>
      <Floor v-for="(floor, id) in floorsCount" :key="'A' + id"/>
    </div>
    <div>
    <Button 
        v-for="(button, id) in floorsButton" :key="id"
        :floorsButton="floorsButton"
        :id="id"
        @buttonClick="addToQueue"
    />
    </div>
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
          floorsCount: 10,
          buttonsCount: 5,
          elevatorsCount: 3,
          height: 125,
          floorsButton: [],
          elevator: [],
          speed: 0,
          tempID: 4,
          queueArr: []
      };
  },
  mounted() {
      for(let i = 0; i < this.buttonsCount; i++) {
        this.floorsButton.push({
          id: i,
          isActive: false
        })
      }
      for(let i = 0; i < this.elevatorsCount; i++) {
        this.elevator.push({
          id: i,
          isActive: false,
          position: 500,
          oldPosition: 500,
          isOpenDoors: false
        })
      }
  },
  methods: {
    addToQueue(id) {
      if(this.tempID != id){
        this.queue(id);
        console.log(id);
        for(let i = 0; i < this.elevator.length; i++) {
          if(!this.elevator[i].isActive && this.queueArr.length > 0) {
            const x = this.queueArr.shift();
            this.move(x, i);
          }
        }
      }
    },

    move(id, i) {
      this.tempID = id;
      this.elevator[i].isActive = true;
      this.elevator[i].oldPosition = this.elevator[i].position;  
      this.elevator[i].position = id * this.height;
      this.speed = Math.abs(this.elevator[i].position - this.elevator[i].oldPosition) / this.height;
      setTimeout(this.openDoors, this.speed * 1000, i);
    },

    openDoors(id) {
        this.elevator[id].isOpenDoors = true;
        setTimeout(this.closeDoors, 3000, id);
    },

    closeDoors(id) {
      this.elevator[id].isOpenDoors = false;
      this.elevator[id].isActive = false;
      this.floorsButton[this.tempID].isActive = false;
      for(let i = 0; i < this.elevator.length; i++) {
            if(!this.elevator[i].isActive && this.queueArr.length > 0) {
              const x = this.queueArr.shift();
              this.move(x, i);
            }
          }
    },

    queue(id) {
      if(!this.floorsButton[id].isActive) {
        this.queueArr.push(id);
        this.floorsButton[id].isActive = true;
        console.log(this.queueArr);
      }
    },
  }
}
</script>
<style lang="scss">
  .house {
    display: flex;
    position: relative;
    margin-top: 25px;
  }
  .shaft {
    display: grid;
    grid-template-columns: 0.1fr 0.1fr;
  }
  .elevator {
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
  .newElevator{
    margin-left: 206px;
  }

</style>