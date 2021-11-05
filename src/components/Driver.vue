<template>
    <div class="driver-container">
        <i v-if="showDropdown" class="fas fa-chevron-down pointer" @click="dropdown($event)"></i>
        #{{driverData["raceNumber"]}}
        <div class="content">
          <ul class="car-drivers">
            <li v-for="driver in driverData['drivers']" :key="driver" class="car-driver">
              <i :class="'fas fa-medal category-' + driver['driverCategory']"></i>
              {{driver['firstName']}} "{{driver['nickName']}}" {{driver['lastName']}}
            </li>
          </ul>
        </div>
    </div>
</template>

<script>
  
  export default {
    props: {
        driverData: Object,
        showDropdown: Boolean
    },
    data() {
      return {}
    },
    methods: {
      dropdown(event) {
        event.target.classList.toggle("active");
        event.target.classList.toggle("fa-chevron-up");
        event.target.classList.toggle("fa-chevron-down");

        var content = event.target.nextSibling.nextSibling;
        if (content.clientHeight) {
          content.style.height = 0;
        } else {
          var wrapper = content.children[0];
          content.style.height = wrapper.clientHeight + "px";
        }
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
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
</style>
