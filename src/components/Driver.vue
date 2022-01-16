<template>
    <div class="driver-container">
      <i class="fas fa-chevron-down pointer left" @click="dropdown($event)"></i>
      #{{driverData["raceNumber"]}}
      <i v-if="showMenu" class="far fa-times-circle pointer right" @click="removeEntry()"></i>
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
              <input class="car-info-select" @change="changeBallast($event)" type="number" min="0" max="100" step="1" :value="driverData['ballastkg'] ?? 0" />
            </div>
            <div class="col-3">Kg</div>
            <span class="tooltip-text">Ballast</span>
          </div>
          <div class="info-tooltip row">
            <div class="col-3"><i class="fas fa-car-battery"></i></div>
            <div v-if="!showMenu" class="col-6">{{driverData['restrictor'] ?? 0}}</div>
            <div v-else class="col-6">
              <input class="car-info-select" @change="changeRestrictor($event)" type="number" min="0" max="100" step="1" :value="driverData['restrictor'] ?? 0" />
            </div>
            <div class="col-3">%</div>
            <span class="tooltip-text">Restrictor</span>
          </div>
          <div :class="'row car-selection-' + driverData['forcedCarModel']">
            <div class="col-3"><i class="fas fa-car"></i></div>
            <div v-if="showMenu" class='col-6'>
              <select class="car-info-select" @input="changeCar($event)">
                <option v-for="element in carList" :key="element" :value="element.key" :selected="element.key == driverData['forcedCarModel'] ? true : null">{{element.value}}</option>
              </select>
            </div>
            <div v-else-if="driverData['forcedCarModel'] != -1" class='col-9'>{{getCarStr(driverData['forcedCarModel'])}}</div>
            <div v-else class="col-9">No car forced!</div>
          </div>
          <div class="row">
            <div class="col-3"><i class="fas fa-user-shield"></i></div>
            <div v-if="!showMenu" class="col-9">{{driverData['isServerAdmin'] ? "Car has admin privileges." : "Car is not an admin."}}</div>
            <div v-else class="col-9"><input type="checkbox" @click="changeAdmin($event)" :checked="driverData['isServerAdmin'] ? true : null"/> Server Admin</div>
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
      return {
        carList: [
          {
            key: "-1",
            value: "Do not force a specific car."
          },
          {
            key: "0",
            value: "2018 Porsche 991 GT3 R"
          },
          {
            key: "1",
            value: "2015 Mercedes-AMG GT3"
          },
          {
            key: "2",
            value: "2018 Ferrari 488 GT3"
          },
          {
            key: "3",
            value: "2015 Audi R8 LMS"
          },
          {
            key: "4",
            value: "2015 Lamborghini Huracan GT3"
          },
          {
            key: "5",
            value: "2015 McLaren 650S GT3"
          },
          {
            key: "6",
            value: "2018 Nissan GT-R Nismo GT3"
          },
          {
            key: "7",
            value: "2017 BMW M6 GT3"
          },
          {
            key: "8",
            value: "2018 Bentley Continental GT3"
          },
          {
            key: "9",
            value: "2017 Porsche 991 II GT3 Cup"
          },
          {
            key: "10",
            value: "2015 Nissan GT-R Nismo GT3"
          },
          {
            key: "11",
            value: "2015 Bentley Continental GT3"
          },
          {
            key: "12",
            value: "2013 AMR V12 Vantage GT3"
          },
          {
            key: "13",
            value: "2017 Reiter Engineering R-EX GT3"
          },
          {
            key: "14",
            value: "2012 Emil Frey Jaguar G3"
          },
          {
            key: "15",
            value: "2016 Lexus RC F GT3"
          },
          {
            key: "16",
            value: "2019 Lamborghini Huracan GT3 Evo"
          },
          {
            key: "17",
            value: "2017 Honda NSX GT3"
          },
          {
            key: "18",
            value: "2015 Lamborghini Huracan SuperTrofeo"
          },
          {
            key: "19",
            value: "2019 Audi R8 LMS Evo"
          },
          {
            key: "20",
            value: "2019 AMR V8 Vantage"
          },
          {
            key: "21",
            value: "2019 Honda NSX GT3 Evo"
          },
          {
            key: "22",
            value: "2019 McLaren 720S GT3"
          },
          {
            key: "23",
            value: "2019 Porsche 911 II GT3 R"
          },
          {
            key: "24",
            value: "2020 Ferrari 488 GT3 Evo"
          },
          {
            key: "25",
            value: "2020 Mercedes-AMG GT3"
          },
          {
            key: "50",
            value: "2018 Alpine A110 GT4"
          },
          {
            key: "51",
            value: "2018 Aston Martin Vantage GT4"
          },
          {
            key: "52",
            value: "2018 Audi R8 LMS GT4"
          },
          {
            key: "53",
            value: "2018 BMW M4 GT4"
          },
          {
            key: "55",
            value: "2017 Chevrolet Camaro GT4"
          },
          {
            key: "56",
            value: "2012 Ginetta G55 GT4"
          },
          {
            key: "57",
            value: "2016 KTM X-Bow GT4"
          },
          {
            key: "58",
            value: "2016 Maserati MC GT4"
          },
          {
            key: "59",
            value: "2016 McLaren 570S GT4"
          },
          {
            key: "60",
            value: "2016 Mercedes AMG GT4"
          },
          {
            key: "61",
            value: "2019 Porsche 718 Cayman GT4 Clubsport"
          }
        ]
      }
    },
    methods: {
      dropdown(event) {
        event.stopPropagation();

        event.target.parentNode.parentNode.classList.toggle("menu-active");
        event.target.classList.toggle("fa-chevron-up");
        event.target.classList.toggle("fa-chevron-down");

        var content = event.target.parentNode.getElementsByClassName("content")[0];
        if (content.clientHeight) {
          content.style.height = 0;
        } else {
          var wrapper = content.children[0];
          content.style.height = wrapper.clientHeight + "px";
        }
      },
      removeEntry() {
        this.$emit('remove', this.driverData);
      },
      changeBallast(event) {
        this.$emit('update-ballastkg', event.target.value);
      },
      changeRestrictor(event) {
        this.$emit('update-restrictor', event.target.value);
      },
      changeCar(event) {
        this.$emit('update-car', event.target.value);
      },
      getCarStr(key) {
        for(var obj of this.carList) {
          if(obj.key == key) {
            return obj.value;
          }
        }
      },
      changeAdmin(event) {
        this.$emit('update-admin', event.target.value);
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .driver-container {
      width: 100%;
    }
    
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

    .car-info-select {
      width: 120%;
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

    .car-selection-9 i {
      color: green;
    }

    .car-selection-18 i {
      color: yellow;
    }

    .car-selection-50 i,
    .car-selection-51 i,
    .car-selection-52 i,
    .car-selection-53 i,
    .car-selection-55 i,
    .car-selection-56 i,
    .car-selection-57 i,
    .car-selection-58 i,
    .car-selection-59 i,
    .car-selection-60 i,
    .car-selection-61 i
    {
        color: blue;
    } 
</style>