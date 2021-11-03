<template>
  <div id="upload col-12">
    <div class="col-12">
      <form enctype="multipart/form-data" novalidate v-if="!FILE_UPLOADED">
        <h1>Upload Entrylist to edit.</h1>
        <div class="dropbox">
          <input type="file" @change="handleUpload($event)" accept=".json,application/json" class="input-file">
            <p>
              Click to browse
            </p>
        </div>
      </form>
      <div v-if="FILE_ERROR">
        <p class="error">
          The uploaded file could not be parsed. Make sure to upload an entry list.
        </p>
      </div>
      <div v-if="FILE_UPLOADED">
        <Editor :qResults="driverList" />
      </div>
    </div>
  </div>
</template>

<script>
  import Editor from "./Editor"
  export default {
    components: {
      Editor
    },
    data() {
      return {FILE_UPLOADED:false, FILE_ERROR:false, parsedResults:null, driverList:[]}
    },
    methods: {
      handleUpload(event) {
        var reader = new FileReader();
        reader.addEventListener("load", () => {
          this.parseJson(reader.result);
        }, false);
        reader.readAsText(event.target.files[0], 'UTF-16LE');
      },
      parseJson(data) {
        this.parsedResults = JSON.parse(data);

        var error = this.checkParsedData();

        if(error) {
          this.FILE_ERROR = true;
           this.driverList = [];
        } else {
          this.driverList = this.parsedResults;
          this.FILE_ERROR = false;
          this.FILE_UPLOADED = !this.FILE_UPLOADED;
        }
      },
      checkParsedData() {
        return this.parsedResults['entries'] == null || this.parsedResults['entries'].length <= 0;
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
    width: 50%;
    }

  .input-file {
    opacity: 0; /* invisible but it's there! */
    width: 100%;
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

  .error {
    color: red;
  }
</style>
