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
        <Driver :driverData="element.item" :showMenu="true" 
          @update-ballastkg="$value => { element.item['ballastkg'] = parseInt($value) }" 
          @update-restrictor="$value => { element.item['restrictor'] = parseInt($value) }" 
          @update-car="$value => { element.item['forcedCarModel'] = parseInt($value) }"
          @update-admin="$value => { element.item['isServerAdmin'] = $value ? 1 : 0}"
          @remove="$value => { driverList.splice(driverList.map((item) => item['item']).indexOf($value), 1); print(driverList.map((item) => item['item']).indexOf($value)) }">
        </Driver>
      </li>
    </template>
  </draggable>
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
        disableDragging: Boolean
    },
    data() {
      return {
        driverList: this.getInitialList(),
        drag: false
      }
    },
    methods: {
      print(val) {
        console.log(val)
      },
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

        document.getElementById('download').setAttribute('href', this.getUTF16Download(list))
      },
      getUTF16Download(data) {
        var str = JSON.stringify(data);

        // ref: https://stackoverflow.com/q/6226189
        var charCode, byteArray = [];

        // LE BOM
        byteArray.push(255, 254);

        for (var i = 0; i < str.length; ++i) {

          charCode = str.charCodeAt(i);
        
          // LE Bytes
          byteArray.push(charCode & 0xff);
          byteArray.push(charCode / 256 >>> 0);
        }
        
        var blob = new Blob([new Uint8Array(byteArray)], {type:'text/plain;charset=UTF-16LE;'});

        return URL.createObjectURL(blob)
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
    color: white!important;
    outline: 2px ridge white;
    outline-offset: -0.25em;
    width: 35%;
    margin-bottom: 1%;
    margin-left: 20%;
    display: flex;
    align-items: center;
  }
  
  .sortable-chosen, .menu-active{
    background: rgba(255, 0, 0, 0.5)!important;
  }

  .flip-list-move {
    transition: transform 0.5s;
  }

  .no-move {
    transition: transform 0s;
  }
</style>

<style scoped>
  .list-group-item {
    cursor: move;
  }

  .list-group-item:nth-child(2n) {
    margin-left: 45%;
  }
</style>
