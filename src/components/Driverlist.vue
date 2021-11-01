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
    @start="drag = true"
    @end="drag = false"
    item-key="order"
  >
    <template #item="{ element }">
      <li class="list-group-item">
        <Driver :driverData="element"></Driver>
      </li>
    </template>
  </draggable>
  <div v-if="showControls">
    <button class="btn btn-secondary button" @click="reset">
      Reset
    </button>
    <button class="btn btn-secondary button" @click="reverse">
      Reverse
    </button>
  </div>
</template>

<script>
  import draggable from 'vuedraggable'
  import Driver from './Driver.vue'

  export default {
    compenents: {
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
        driverList: this.initialList.copy(),
        drag: false
      }
    },
     methods: {
      reset() {
        this.driverList = this.initialList.copy();
      },
      reverse() {
        this.driverList = this.driverList.reverse();
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

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .list-group {
    min-height: 20px;
  }
  .list-group-item {
    cursor: move;
  }
  .flip-list-move {
    transition: transform 0.5s;
  }
  .no-move {
    transition: transform 0s;
  }
</style>
