<template>
  <div class="container col-12">
    <div class="col-6">
      <Driverlist :initialList="qResults" :showControls="false" :draggingDisabled="false"></Driverlist>
    </div>
    <div class="col-6">
      <Driverlist :initialList="qResults" :showControls="true" :draggingDisabled="true"></Driverlist>
    </div>
  </div>
</template>

<script>
  import Driverlist from './Driverlist.vue'
  
  export default {
    compenents: {
      Driverlist,
    },
    props: {
        qResults: Object
    },
    data() {
      return {}
    },
  }
</script>