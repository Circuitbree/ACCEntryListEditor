<template>
    <div class="driver-container">
        <i v-if="showDropdown" class="fas fa-chevron-down" @click="dropdown()"></i>
        {{driverData["raceNumber"]}}
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
      dropdown(){
        console.log("yep")
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    i {
        cursor: pointer;
    }
</style>
