<template>
  <div id="upload">
    <div class="container">
      <form enctype="multipart/form-data" novalidate v-if="!FILE_UPLOADED">
        <h1>Upload Qualification Results</h1>
        <div class="dropbox">
          <input type="file" :name="qResultsUpload" @change="handleUpload($event)" accept=".json,application/json" class="input-file">
            <p>
              Click to browse
            </p>
        </div>
      </form>
      <div v-if="FILE_UPLOADED">
        <Editor></Editor>
      </div>
    </div>
  </div>
</template>

<script>
  import Editor from "./Editor.vue"
  export default {
    components: {
      Editor
    },
    data() {
      return {FILE_UPLOADED:false, parsedResults:null}
    },
    methods: {
      handleUpload(event) {
        this.FILE_UPLOADED = !this.FILE_UPLOADED;
        event.target.files[0].text().then((value) => {this.parseJson(value)})
      },
      parseJson(data) {
        this.parsedResults = JSON.parse(data);

        // todo error check
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .dropbox {
    outline: 2px dashed grey; /* the dash box */
    outline-offset: -10px;
    background: lightcyan;
    color: dimgray;
    padding: 10px 10px;
    min-height: 200px; /* minimum height */
    position: relative;
    cursor: pointer;
  }

  .input-file {
    opacity: 0; /* invisible but it's there! */
    width: 50%;
    height: 200px;
    position: absolute;
    cursor: pointer;
  }

  .dropbox:hover {
    background: lightblue; /* when mouse over to the drop zone, change color */
  }

  .dropbox p {
    font-size: 1.2em;
    text-align: center;
    padding: 50px 0;
  }
</style>
