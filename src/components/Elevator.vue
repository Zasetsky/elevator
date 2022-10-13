<template>
  <div class="shaft">
    <div 
      class="shaft__elevator"
      :class="{openDoors: isOpenDoors}" 
      :style="{marginTop: position + 'px', transition: speed + 's'}">
    </div>
    <Floor 
    v-for="(item, id) in floors" :key="id"
    :floorsButton="floors"
    :id="id"
    @buttonClick="move"
    />
  </div>
</template>
<script>
import Floor from './Floor.vue'

export default {
    name: "ElevatorComponent",
    components: { Floor },
    data() {
        return {
            isActiveElevator: false,
            floorsCount: 5,
            isOpenDoors: false,
            position: 500,
            height: 125,
            oldPosition: 500,
            floors: []
        };
    },
    computed: {
        speed() {
            return Math.abs(this.position - this.oldPosition) / this.height;
        },
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
        move(id) {
            this.oldPosition = this.position;
            if (!this.isActiveElevator && !this.isOpenDoors) {
                this.isActiveElevator = true;
                
                this.position = id * this.height;
                setTimeout(this.deactivatingElevator, this.speed * 1000);
                setTimeout(this.openDoors, this.speed * 1000);
            }
        },

        deactivatingElevator() {
            this.isActiveElevator = false;
        },

        openDoors() {
            this.isOpenDoors = true;
            setTimeout(this.closeDoors, 3000);
        },

        closeDoors() {
            this.isOpenDoors = false;
        }
    }
}
</script>
<style lang="scss">
.shaft {

  &__elevator {
    position: absolute;
    width: 100px;
    height: 100px;
    margin-left: 50px;
    background-color: red; 
  }
}
.openDoors {
  background-color: yellow;
  transition: all 1s !important;
}

</style>