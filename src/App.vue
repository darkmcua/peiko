<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/peiko-logo-mini.png">
    <h1>Test by Korniyko</h1>
    <hr>

    <div class="wrap">

      <button @click="showTableFunc" v-if="!showTable">{{ textStatus == null ? status.start : textStatus }}</button>

      <div v-if="showTable" class="divTable">

        <div class="divTableHeading">
          <div class="divTableRow">
            <div class="divTableHead">
              <strong>Stock</strong>
            </div>
            <div class="divTableHead">
              <strong>Current</strong>
            </div>
            <div class="divTableHead">
              <strong>Change</strong>
            </div>
          </div>
        </div>

        <div class="divTableBody">
          <div class="divTableRow" v-for="item in sortTable()" :key="item.name">
            <div class="divTableCell">
              {{ item.name }}
            </div>
            <div class="divTableCell">
              {{ item.current.toFixed(2) }}
            </div>
            <div class="divTableCell" :style="item.current < item.start ? 'color: red;': 'color: #06bc06;'">
              {{item.current > item.start ? '+' : ''}}{{ changeResult(item.current,item.start) }}
            </div>
          </div>
        </div>

      </div>
    </div>
  </div>

</template>

<script>
import simulateAsyncReq from '@/plugins/getDataFunc';
import payload from "@/mocData";

export default {
  name: 'App',
  data () {
    return {
      showTable: false,
      status: {
        start: 'Get data',
        loading: 'Loading...',
        error: 'No data! Click again.'
      },
      textStatus: null,
      result: null
    }
  },
  methods: {
    showTableFunc(){
      this.textStatus = this.status.loading;
      simulateAsyncReq(payload)
          .then((result) => {
            (
                this.result = result,
                this.showTable = true
            )
          })
          .catch((err) => {
            console.log(err);
            this.textStatus = this.status.error;
          })
    },
    sortTable() {
      return this.result.slice().sort(function(a, b){
        return (a.name > b.name) ? 1 : -1;
      })
    },
    changeResult(a, b){
      return (((a - b) / ((a + b) / 2)) * 100).toFixed(2);
    }
  }
}
</script>

<style lang="scss" scoped>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
