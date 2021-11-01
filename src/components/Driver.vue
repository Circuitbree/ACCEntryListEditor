<template>
    <div class="driver-container">
        <i></i>
        {{driverData["raceNumber"]}}
    </div>
</template>

<script>
  
  export default {
    props: {
        driverData: Object
    },
    data() {
      return {}
    },
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    i {
        cursor: pointer;
    }
</style>
