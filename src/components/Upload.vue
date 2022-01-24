<template>
  <div class="upload row">
    <div class="col-12">
      <form enctype="multipart/form-data" novalidate v-if="!FILE_UPLOADED">
        <div class="dropbox offset-3 col-6">
          <p class="info">
            Upload Entrylist or Race Results <i class="fa fa-upload" aria-hidden="true"></i>
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
        <div class="row"> 
          <div class="col-12">
            <button class="btn btn-secondary button" @click="resetEditor()">
              Edit another Entrylist <i class="fas fa-edit"></i>
            </button>
          </div>
        </div>
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
      return {
          FILE_UPLOADED:false, 
          FILE_ERROR:false, 
          TYPE_ENTRYLIST: 'Entry',
          TYPE_RESULTS: 'Results',
          FILE_ENCODING: 'UTF-16LE',
          ENTRIES_STR: 'entries',
          SESSION_RES_STR: 'sessionResult',
          LEADERBOARD_STR: 'leaderBoardLines',
          parsedResults:null, 
          driverList:[]
        }
    },
    methods: {
      handleUpload(event) {
        var reader = new FileReader();
        reader.addEventListener("load", () => {
          this.parseJson(reader.result);
        }, false);
        reader.readAsText(event.target.files[0], this.FILE_ENCODING);
      },
      parseJson(data) {
        this.parsedResults = JSON.parse(data);

        var parseRes = this.checkParsedData();

        if(parseRes.error) {
          this.FILE_ERROR = true;
           this.driverList = [];
        } else {
          this.FILE_ERROR = false;
          this.FILE_UPLOADED = !this.FILE_UPLOADED;

          if(parseRes.type == this.TYPE_ENTRYLIST) {
            this.driverList = this.parsedResults;

            // sort list by defaultGridPosition to load correct order insted of join order
            this.driverList[this.ENTRIES_STR].sort((a, b) => (a.defaultGridPosition > b.defaultGridPosition) ? 1 : ((b.defaultGridPosition > a.defaultGridPosition) ? -1 : 0))
          } else if(parseRes.type == this.TYPE_RESULTS) {
            // got a results file, init an empty entrylist
            this.driverList = {entries: [], configVersion: 1, forceEntryList: 0};

            // fill in drivers and positions from results file
            var leaderboard = this.parsedResults[this.SESSION_RES_STR][this.LEADERBOARD_STR];
            for(var i = 0; i < leaderboard.length; i++) {
              var car = leaderboard[i];
              this.driverList.entries.push({
                drivers: [],
                customCar: "",
                raceNumber: car.car.raceNumber,
                defaultGridPosition: i + 1,
                forcedCarModel: car.carModel,
                overrideDriverInfo: 0,
                isServerAdmin: 0,
                overrideCarModelForCustomCar: 1,
                configVersion: 1,
                laps: car.timing.lapCount
              });

              for(var driver_i = 0; driver_i < car.car.drivers.length; driver_i++)
              {
                this.driverList.entries[i].drivers.push({"playerID": car.car.drivers[driver_i].playerId});
              }
            }
          }

          // fix for 1.8 include forcecarmodel if not already present

          for(var j = 0; j < this.driverList.entries.length; j++) {
            if(!this.driverList.entries[j].forcedCarModel)
              this.driverList.entries[j].forcedCarModel = -1
          }
        }
      },
      checkParsedData() {
        var res = {
          error: null, 
          type: ''
        }

        if(this.parsedResults.length <= 0 || (!this.parsedResults[this.ENTRIES_STR] && !this.parsedResults[this.SESSION_RES_STR])) {
          res.error = true;
          return res;
        }

        res.error = false;

        if(this.parsedResults[this.ENTRIES_STR]) {
          res.type = this.TYPE_ENTRYLIST;
          return res;
        }

        if(this.parsedResults[this.SESSION_RES_STR]) {
          res.type = this.TYPE_RESULTS;
          return res;
        }
      },
      resetEditor() {
        this.FILE_UPLOADED = false
        this.FILE_ERROR = false
        this.parsedResults = null 
        this.driverList = []
      }
    }
  }
</script>

<style scoped>
  .btn {
    margin: 0 3%;
    cursor: pointer;
    border: 0;
    border-radius: 0;
    background: rgba(0, 0, 0, 0.5);
    color: white;
    outline: 2px ridge white;
    outline-offset: -0.35em;
    width: 94%;
  }

  .btn:hover {
    background: white;
    color: black;
    outline-color: lightskyblue;
  }

  .btn:hover i {
    color: lightskyblue;
  }

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
