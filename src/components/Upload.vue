<template>
  <div class="upload row">
    <div class="col-12">
      <form enctype="multipart/form-data" novalidate v-if="!FILE_UPLOADED">
        <div class="dropbox offset-3 col-6">
          <p class="info">
            Upload Entrylist <i class="fa fa-upload" aria-hidden="true"></i>
          </p>
          <input type="file" @change="handleUpload($event)" accept=".json,application/json" class="input-file" title=" ">
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

          // sort list by defaultGridPosition to load correct order insted of join order
          this.driverList['entries'].sort((a, b) => (a.defaultGridPosition > b.defaultGridPosition) ? 1 : ((b.defaultGridPosition > a.defaultGridPosition) ? -1 : 0))
        }
      },
      checkParsedData() {
        return this.parsedResults['entries'] == null || this.parsedResults['entries'].length <= 0;
      }
    }
  }
</script>

<style scoped>
  .upload {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100%;
    padding: 0;
    margin: 0;
  }

  .dropbox {
    outline: 2px ridge white;
    outline-offset: -0.75em;
    background: rgba(0, 0, 0, 0.5);
    min-height: 150px;
    position: relative;
    cursor: pointer;
    overflow: hidden;
    color: white;
  }

  .input-file {
    opacity: 0; 
    width: 100%;
    height: 100%;
    cursor: pointer;
    min-height: 150px;
    position: absolute;
    left: 0px;
    top: 0px;
  }

  .dropbox:hover {
    background: white;
    color: black;
    outline-color: lightskyblue;
  }

  .dropbox:hover .info i {
    color: lightskyblue;
  }

  .dropbox p {
    font-size: 1.1em;
    text-align: center;
    padding: 3.5em 0;
    vertical-align: middle;
    min-height: 150px;
    margin-bottom: 0;
  }

  .error {
    color: red;
  }
</style>
