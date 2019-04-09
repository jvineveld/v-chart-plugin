<template>
  <div class="container">
    <div class="row">
      <div class="col">
        <img class = "logo" src="../assets/img/logo.png">
        </div>
    </div>

    <div class="row">
      <div class="form-group col-6 col-md-4">
        <div v-for="(t, index) in sales">
            <input v-model.number="sales[index].total" type="number"> 
            <button v-model="sales[index]" type="submit" @click="removeItem(index, $event)"> [-] </button>             
        </div>
        <button v-on:click="newItem"> [+] </button>
      </div>
      <div class="col-6 col-md-8">
        <div class="row">
          <div class="col-12" id="drawingboard">
            <v-chart v-bind:chartData="scatterPlotData"></v-chart>
          </div>
        </div>  
      </div>
    </div>
    <a href="https://github.com/ignoreintuition/v-chart-plugin"><img style="position: absolute; top: 0; right: 0; border: 0;" src="https://s3.amazonaws.com/github/ribbons/forkme_right_red_aa0000.png" alt="Fork me on GitHub"></a>
  </div>
</template>

<script>
import sales from "../assets/data/sales";
import * as d3 from 'd3'

export default {
  name: "barChartExample",
  mounted(){
    const el = d3.select('#drawingboard svg')
    console.log('jej', el)
		el.on('painting', (attr1, attr2, attr3) => {
      console.log(attr1, attr2, attr3)
    })
  },
  methods: {
    newItem: function() {
      this.sales.push({
        month: null,
        year: null,
        total: null,
        actual: false
      });
    },
    removeItem: function(d, e) {
      e.preventDefault();
      this.sales.splice(d, 1);
    }
  },
  data() {
    return {
      sales: sales,
      scatterPlotData: {
        chartType: "paintingPlot",
        selector: "scatterPlot",
        title: "Scatter Plot",
        subtitle: "Sales by month",
        width: 600,
        height: 500,
        dim: "month",
        label: {
          enabled: true,
        },
        metric: ['total', 'forecast'],
        data: sales,
      },
    };
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.logo {
  width: 200px;
}
</style>
