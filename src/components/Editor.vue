<template>
  <div class="row controls">
    <div class="margin-bottom-1">
      <button class="btn btn-secondary button" @click="$refs.editList.reset()">
        Reset
      </button>
      <button class="btn btn-secondary button" @click="$refs.editList.reverse()">
        Reverse
      </button>
      <a id="download" download="entrylist.json" class="btn btn-secondary button" @mousedown="$refs.editList.download()">
        Download <i class="fa fa-download" aria-hidden="true"></i>
      </a>
    </div>
  </div>
  <div class="row">
    <div class="col-6">
      <h3>Original Order</h3>
      <ul class="list-group">
        <li class="list-group-item" v-for="element in originalList" :key="element">
          <Driver :driverData="element" :showMenu="false"></Driver>
        </li>
      </ul>
    </div>
    <div class="col-6">
      <h3>Edit Order</h3>
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
      return {
        originalList: JSON.parse(JSON.stringify(this.qResults['entries']))
      }
    },
  }
</script>

<style scoped>
  .margin-bottom-1 {
    margin-bottom: 1%;
  }

  .controls {
    margin-top: 1%;
  }

  .controls .btn {
    cursor: pointer;
    border: 0;
    border-radius: 0;
    background: rgba(0, 0, 0, 0.5);
    color: white;
    outline: 2px ridge white;
    outline-offset: -0.35em;
  }

  .controls .btn:hover {
    background: white;
    color: black;
    outline-color: lightskyblue;
  }

  .controls .btn:hover i {
    color: lightskyblue;
  }

  .controls .btn {
    margin: 0 1%;
    width: 30%;
  }

  h3 {
    color: white;
  }
</style>