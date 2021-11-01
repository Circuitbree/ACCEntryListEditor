<template>
  <div class="container col-12">
    <div class="col-6">
      <template v-for="element in qResults" :key="element">
        <li class="list-group-item">
          <Driver :driverData="element"></Driver>
        </li>
      </template>
    </div>
    <div class="col-6">
      <Driverlist :initialList="qResults" :showControls="true" :disableDragging="false"/>
    </div>
  </div>
</template>

<script>
  import Driverlist from './Driverlist.vue'
  
  export default {
    components: { Driverlist },
    props: {
        qResults: Object
    },
    data() {
      return {}
    },
  }
</script>

<style scoped>
  .list-group {
    min-height: 20px;
  }
</style>