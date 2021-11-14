<template>
  <div class="row controls">
    <div class="margin-bottom-2">
      <button class="btn btn-secondary button" @click="$refs.editList.reset()">
        Reset
      </button>
      <button class="btn btn-secondary button" @click="$refs.editList.reverse()">
        Reverse
      </button>
      <a id="download" download="entrylist.json" class="btn btn-secondary button" @mousedown="$refs.editList.download()">
        <i class="fa fa-download" aria-hidden="true"></i> Download
      </a>
    </div>
  </div>
  <div class="row">
    <div class="col-6">
      <ul class="list-group">
        <li class="list-group-item" v-for="element in qResults['entries']" :key="element">
          <Driver :driverData="element" :showMenu="false"></Driver>
        </li>
      </ul>
    </div>
    <div class="col-6">
      <Driverlist ref="editList" :initialList="qResults" :disableDragging="false"/>
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

<style scoped>
  .margin-bottom-2 {
    margin-bottom: 2%;
  }

  .controls .btn {
    margin: 1%;
    width: 30%;
  }
</style>