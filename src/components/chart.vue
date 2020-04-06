<template>
  <div class="graph">
    <v-chart
      v-if="assetData.data.length > 0"
      v-bind:chartData="{
        ...chartData,
        chartType: 'lineChart' ,
        selector: 'asset' + this.title ,
        title: 'Assets',
        height: 120,
        minX: 500 ,
        overrides: {
          x: { ticks: 500 },
        },
        ...assetData
      }"
    ></v-chart>
  </div>
</template>

<script>
export default {
  //glData: store.$state,
  name: "example",
  props: ["title", "measureid", "width", "height"],
  data: function() {
    return {
      chartData: {
        chartType: "vLineChart",
        title: this.title,
        width: this.width,
        height: this.height,
        dim: "time",
        metric: "measure"
      }
    };
  },
  computed: {

    becalmData() {
      let gData = [];
      for (let timeStamp in this.$store.state.dbBecalm[this.measureid]) {
        let measure = this.$store.state.dbBecalm[this.measureid][timeStamp];
        if (measure > 0) gData.push({ acc: timeStamp, balance: measure });
      }
      return { data: gData };
      //return this.$store.getters.moneyData;
    },
  }
};
</script>

<style>
.graph {
  display: inline-block;
  *display: inline;
  margin-left: 20px;
  vertical-align: bottom;
}
</style>
