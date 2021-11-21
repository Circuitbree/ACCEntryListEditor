<template>
    <div class="driver-container">
        <i v-if="showMenu" class="fas fa-bars left"></i>
        #{{driverData["raceNumber"]}}
        <i class="fas fa-chevron-down pointer right" @click="dropdown($event)"></i>
        <div class="content">
          <div class="wrapper">
            <div>
              <b>Drivers: </b>
              <ul class="car-drivers">
                <li v-for="driver in driverData['drivers']" :key="driver" class="car-driver">
                  <i :class="'fas fa-medal category-' + driver['driverCategory']"></i>
                  {{driver['firstName']}} "{{driver['nickName']}}" {{driver['lastName']}}
                </li>
              </ul>
            </div>
            <b>Car Information: </b>
            <div class="info-tooltip row">
              <div class="col-3"><i class="fas fa-weight-hanging"></i></div>
              <div v-if="!showMenu" class="col-6">{{driverData['ballastkg'] ?? 0}}</div>
              <div v-else class="col-6">
                <input @change="changeBallast($event)" type="number" min="0" max="100" step="1" :value="driverData['ballastkg'] ?? 0" />
              </div>
              <div class="col-3">Kg</div>
              <span class="tooltip-text">Ballast</span>
            </div>
            <div class="info-tooltip row">
              <div class="col-3"><i class="fas fa-car-battery"></i></div>
              <div v-if="!showMenu" class="col-6">{{driverData['restrictor'] ?? 0}}</div>
              <div v-else class="col-6">
                <input @change="changeRestrictor($event)" type="number" min="0" max="100" step="1" :value="driverData['restrictor'] ?? 0" />
              </div>
              <div class="col-3">%</div>
              <span class="tooltip-text">Restrictor</span>
            </div>
            <div class="row">
              <div class="col-3"><i class="fas fa-car"></i></div>
              <div v-if="showMenu" :class="'col-9 car-selection-' + driverData['forcedCarModel']"></div>
              <div v-else-if="driverData['forcedCarModel'] != -1" :class="'col-9 car-selection-' + driverData['forcedCarModel']"></div>
              <div v-else class="col-9">No car forced!</div>
            </div>
          </div>
        </div>
    </div>
</template>

<script>
  
  export default {
    props: {
        driverData: Object,
        showMenu: Boolean
    },
    data() {
      return {}
    },
    methods: {
      dropdown(event) {
        event.stopPropagation();

        event.target.parentNode.parentNode.classList.toggle("menu-active");
        event.target.classList.toggle("fa-chevron-up");
        event.target.classList.toggle("fa-chevron-down");

        var content = event.target.nextSibling;
        if (content.clientHeight) {
          content.style.height = 0;
        } else {
          var wrapper = content.children[0];
          content.style.height = wrapper.clientHeight + "px";
        }
      },
      changeBallast(event) {
        this.$emit('update-ballastkg', event.target.value);
      },
      changeRestrictor(event) {
        this.$emit('update-restrictor', event.target.value);
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .left {
      float: left;
    }

    .right {
      float: right;
    }

    .pointer {
        cursor: pointer;
    }

    .content {
      height: 0;
      overflow: hidden;
      transition: height 0.3s ease-out;
    }

    .category-3 {
      color: #e5e4ff; /* Platinum */
    }

    .category-2 {
      color: gold;
    }

    .category-1 {
      color: silver;
    }

    .category-0 {
      color: #CD7F32; /* Bronze */
    }

    .car-drivers li {
      list-style: none;
    }

    .car-drivers {
      padding: 0;
    }

    .info-tooltip .tooltip-text {
      visibility: hidden;
      width: 90px;
      background-color: black;
      color: #fff;
      text-align: center;
      border-radius: 6px;
      margin-left: 85%;

      /* Position the tooltip */
      position: absolute;
      z-index: 1;
    }

    .info-tooltip:hover .tooltip-text {
      visibility: visible;
    }
</style>