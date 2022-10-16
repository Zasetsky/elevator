<template>
  <div>
    <div class="house">
      <div class="house__shaft">
        <div 
          v-for="el in elevator" :key="el.id"
          class="house__shaft__elevator"
          :class="{ openDoors: el.isOpenDoors }" 
          :style="{
            marginTop: el.position + 'px',
            transition: speed + 's',
            marginLeft: el.margin + 'px',
            height: elevatorHeight + 'px',
            width: elevatorWidth + 'px'
          }"
        >
          <p v-if="el.isUp">
            {{ el.floorNumber }} &#11014; 
          </p> 
          <p v-if="el.isDown">
            {{ el.floorNumber }} &#11015; 
          </p>
        </div>
        <Floor 
          v-for="(floor, id) in floorsCount" :key="'A' + id"
          :class="getFloorClasses()"
        />
      </div>
      <div>
        <Button 
            v-for="(button, id) in floorsButton" :key="id"
            :floorsButton="floorsButton"
            :id="id"
            :class="getButtonClasses()"
            @buttonClick="addToQueue"
        />
      </div>
    </div>
    <button @click="fiveFloors()">5 этажей</button>
    <button @click="tenFloors()">10 этажей</button>
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
          floorsCount: 30,
          buttonsCount: 10,
          elevatorsCount: 3,
          height: 67,
          elevatorHeight: 40,
          elevatorWidth: 21,
          speed: 0,
          queueArr: [],
          floorsButton: [],
          elevator: [],
      };
  },
 
  created() {
    if(localStorage.getItem('elevator')) {
      this.elevator = JSON.parse(localStorage.getItem("elevator"))
      for(let i = 0; i < this.elevator.length; i++) {
        this.elevator[i].isActive = false;
        this.elevator[i].isOpenDoors = false;
        this.elevator[i].isUp = false;
        this.elevator[i].isDown = false;
      }
    } else {
          for(let i = 0; i < this.elevatorsCount; i++) {
            this.elevator.push({
              id: i,
              tempId: 4,
              isActive: false,
              position: 603,
              oldPosition: 603,
              isOpenDoors: false,
              isUp: false,
              isDown: false,
              floorNumber: null,
              margin: 50
            })
          }
        }
    this.elevator[1].margin = 137
    this.elevator[2].margin = 225    

    for(let i = 0; i < this.buttonsCount; i++) {
      this.floorsButton.push({
        id: i,
        isActive: false,
        isBusy: false
      })
    }
  },

  methods: {
    addToQueue(id) {
      if(!this.floorsButton[id].isActive && !this.floorsButton[id].isBusy) {
        this.queue(id);

        for(let i = 0; i < this.elevator.length; i++) {
          if(!this.elevator[i].isActive && this.queueArr.length > 0) {
            
            const x = this.queueArr.shift();
            this.move(x, i);
          }
        }
      }
    },

    move(id, i) {
      this.elevator[i].isActive = true;

      this.setPosition(id, i);
      this.setSpeed(i);
      this.upOrDown(i);
      this.floorCounter(id, i);
      setTimeout(this.openDoors, this.speed * 1000, id, i);
      if(this.buttonsCount == 10) {
        this.savePosition()
      } else {
        this.savePosition2()
      }
      
    },

    setPosition(id, i) {
      this.elevator[i].oldPosition = this.elevator[i].position;  
      this.elevator[i].position = id * this.height;

      this.idSave(id, i);
    },

    setSpeed(i) {
      this.speed = Math.abs(this.elevator[i].position - this.elevator[i].oldPosition) / this.height;
    },
    
    openDoors(id, i) {
      this.elevator[i].isOpenDoors = true;
      this.floorsButton[id].isActive = false;
      this.elevator[i].isUp = false;
      this.elevator[i].isDown = false;
      setTimeout(this.closeDoors, 3000, id, i);
    },

    closeDoors(id, i) {
      this.elevator[i].isOpenDoors = false;
      this.elevator[i].isActive = false;

      for(let i = 0; i < this.elevator.length; i++) {
        if(!this.elevator[i].isActive && this.queueArr.length > 0) {
          const x = this.queueArr.shift();
          this.move(x, i);
        }
      }
    },

    upOrDown(i) {
      if(this.elevator[i].position < this.elevator[i].oldPosition) {
        this.elevator[i].isUp = true;
      } else {
        this.elevator[i].isDown = true;
      }
    },

    floorCounter(id, i) {
      this.elevator[i].floorNumber = this.buttonsCount - id;
    },

    idSave(id, i) {
      this.floorsButton[this.elevator[i].tempId].isBusy = false;    
      this.elevator[i].tempId = id;
    },

    savePosition() {
      const parsed = JSON.stringify(this.elevator);
      localStorage.setItem('elevator', parsed);
    },

    savePosition2() {
      const parsed = JSON.stringify(this.elevator);
      localStorage.setItem('elevatorFive', parsed);
    },

    queue(id) {
      if(!this.floorsButton[id].isActive) {
        this.queueArr.push(id);

        this.floorsButton[id].isActive = true;
        this.floorsButton[id].isBusy = true;
        console.log(this.queueArr);
      }
    },

    fiveFloors() {
      this.buttonsCount = 5;
      this.floorsCount = 15;
      this.speed = 0;

      
      for(let i = 0; i < this.elevator.length; i++) {
        this.elevator[i].isActive = false;
        this.elevator[i].isOpenDoors = false;
        this.elevator[i].isUp = false;
        this.elevator[i].isDown = false;
      }

      if(localStorage.getItem('elevatorFive')) {
        this.elevator = JSON.parse(localStorage.getItem("elevatorFive"))
        for(let i = 0; i < this.elevator.length; i++) {
          this.elevator[i].isActive = false;
          this.elevator[i].isOpenDoors = false;
          this.elevator[i].isUp = false;
          this.elevator[i].isDown = false;
        }
      } else {
          for(let i = 0; i < this.elevator.length; i++) {
            this.elevator[i].position = 500;
            this.elevator[i].oldPosition = 500;
          }
        }

      this.height = 125;
      this.elevator[1].margin = 217;
      this.elevator[2].margin = 382;
      this.elevatorHeight = 100;
      this.elevatorWidth = 100;

      this.floorsButton = []
      for(let i = 0; i < this.buttonsCount; i++) {
        this.floorsButton.push({
          id: i,
          isActive: false,
          isBusy: false
        })
      }
    },

    tenFloors() {
      this.buttonsCount = 10;
      this.floorsCount = 30;
      this.speed = 0;

      for(let i = 0; i < this.elevator.length; i++) {
        this.elevator[i].isActive = false;
        this.elevator[i].isOpenDoors = false;
        this.elevator[i].isUp = false;
        this.elevator[i].isDown = false;
      }

      if(localStorage.getItem('elevator')) {
      this.elevator = JSON.parse(localStorage.getItem("elevator"))
      for(let i = 0; i < this.elevator.length; i++) {
        this.elevator[i].isActive = false;
        this.elevator[i].isOpenDoors = false;
        this.elevator[i].isUp = false;
        this.elevator[i].isDown = false;
      }
      } else {
          for(let i = 0; i < this.elevator.length; i++) {
            this.elevator[i].position = 603;
            this.elevator[i].oldPosition = 603;
          }
        }

      this.height = 67;
      this.elevatorHeight = 40;
      this.elevatorWidth = 21;
      this.elevator[1].margin = 137;
      this.elevator[2].margin = 225 ;

      this.floorsButton = []
      for(let i = 0; i < this.buttonsCount; i++) {
        this.floorsButton.push({
          id: i,
          isActive: false,
          isBusy: false
        })
      }
    },

    getButtonClasses() {
      const classes = [];
      if(this.buttonsCount < 10) {
        classes.push('fiveButtons');
      }
      return classes;
    },

    getFloorClasses() {
      const classes = [];
      if(this.floorsCount <= 15) {
        classes.push('fiveFloors');
      }
      return classes;
    },

  }
}
</script>
<style lang="scss">
  .house {
    display: flex;
    position: relative;
    margin-top: 25px;

    &__shaft {
      display: grid;
      grid-template-columns: 0.1fr 0.1fr 0.1fr;

      &__elevator {
        position: absolute;
        background-color: #76e6ff;
        font-size: 25px;
        
        p {
          text-align: center;
          margin-top: -25px;
        }
      }
    }
  }

  @keyframes blinking {
    0% { background: #52defeda }
    50% { background: #aff0ff }
    100% { background: #3fbeda }
  }

  .openDoors {
    animation: blinking 1000ms infinite;
  }

  .newElevator{
    margin-left: 137px;
  }

  .newElevator2{
    margin-left: 225px;
  }
</style>