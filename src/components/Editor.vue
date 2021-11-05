<template>
  <div class="container row">
    <div class="col-6">
      <ul class="list-group">
        <li class="list-group-item" v-for="element in qResults['entries']" :key="element">
          <Driver :driverData="element" :showDropdown="false"></Driver>
        </li>
      </ul>
    </div>
    <div class="col-6">
      <Driverlist :initialList="qResults" :showControls="true" :disableDragging="false"/>
    </div>
  </div>
</template>

<script>
  import Driverlist from './Driverlist.vue'
  import Driver from './Driver.vue'
  
  export default {
    components: { Driverlist, Driver },
    props: {
        qResults: Object
    },
    data() {
      return {}
    },
  }
</script>