<template>
  <draggable
    class="list-group"
    tag="transition-group"
    :component-data="{
      tag: 'ul',
      type: 'transition-group',
      name: !drag ? 'flip-list' : null
    }"
    v-model="driverList"
    v-bind="dragOptions"
    @start="drag = !disableDragging"
    @end="drag = false"
    item-key="order"
  >
    <template #item="{ element }">
      <li class="list-group-item">
        <Driver :driverData="element.item" :showDropdown="true"></Driver>
      </li>
    </template>
  </draggable>
  <div v-if="showControls" class="row">
    <button class="btn btn-secondary button col-4" @click="reset">
      Reset
    </button>
    <button class="btn btn-secondary button col-4" @click="reverse">
      Reverse
    </button>
    <a id="download" download="entrylist.json" class="btn btn-secondary button col-4" @mousedown="download">
      <i class="fa fa-download" aria-hidden="true"></i> Download
    </a>
  </div>
</template>

<script>
  import draggable from 'vuedraggable'
  import Driver from './Driver.vue'

  export default {
    components: {
      draggable,
      Driver,
    },
    props: {
        initialList: Object,
        showControls: Boolean,
        disableDragging: Boolean
    },
    data() {
      return {
        driverList: this.getInitialList(),
        drag: false
      }
    },
    methods: {
      reset() {
        this.driverList = this.getInitialList();
      },
      reverse() {
        this.driverList = this.driverList.reverse();
      },
      download(){
        var list = JSON.parse(JSON.stringify(this.initialList))

        list['entries'] = this.driverList.map((item) => item["item"])
        
        for(var car of list['entries']) {
          car['defaultGridPosition'] = list['entries'].indexOf(car) + 1;
        }

        document.getElementById('download').setAttribute('href', "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(list)))
      },
      getInitialList() {
        return this.initialList['entries'].slice().map((item, index) => {
            return { item, order: index + 1 }; 
          }
        )
      }
    },
    computed: {
      dragOptions() {
        return {
          animation: 200,
          group: "description",
          disabled: false,
          ghostClass: "ghost"
        };
      }
    }
  }
</script>

<style>
  .list-group {
    min-height: 20px;
  }

  .list-group-item {
    background: rgba(0, 0, 0, 0.5)!important;
    cursor: move;
    color: white!important;
    outline: 2px ridge white;
    outline-offset: -0.25em;
  }
  
  .sortable-chosen{
    background: rgba(255, 0, 0, 0.5)!important;
  }

  .flip-list-move {
    transition: transform 0.5s;
  }

  .no-move {
    transition: transform 0s;
  }
</style>
