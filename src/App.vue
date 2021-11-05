<template>
  <Upload></Upload>
</template>

<script>
import Upload from './components/Upload.vue'

export default {
  name: 'App',
  components: {
    Upload
  }
}
</script>

<style>

html, body {
    height: 100%;
}
body {
    margin: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  height: 100%;
}
</style>
