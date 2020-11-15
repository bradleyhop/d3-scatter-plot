<script>
// import * as d3 from 'd3';

export default {
  name: 'ScatterPlot',

  data() {
    return {
      cycleData: undefined, // placeholder for fetch'ed gdp data
      widthChart: 1000, // width of svg area
      heightChart: 500, // height of svg area
      padding: 65, // padding of chart
    };
  },

  mounted() {
    fetch('https://raw.githubusercontent.com/freeCodeCamp/ProjectReferenceData/master/cyclist-data.json')
      .then((response) => response.json())
      .then((data) => {
        this.cycleData = data;
      })
      .then(() => console.log(this.cycleData))
      // .then(() => this.init())
      .catch((error) => console.log(error));
  },

  methods: {
    // called by mounted() after async data is fetch'ed
    init() {
      console.log(this.cycleData);
    },

  },
};
</script>

<template>
  <div class="container-bar-chart">
    <h2 id="title" class="chart-title">
      Thrity-Five Fastest Times Up Alpe d'Huez
    </h2>
    <div id="bar-chart" class="bar-chart">
    </div>
  </div>
</template>

<style lang="scss">
/*
 * NB: Do not scope this component's style! D3 won't be able to see it!!!
 */

.container-bar-chart {
  width: 1050px;
  height: 550px;
  margin: auto;
  background-color: $chart-background;
  border-radius: 15px;
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16), 0 3px 6px rgba(0, 0, 0, 0.23);
}

.chart-title {
  padding-top: 1rem;
  margin-bottom: 0;
  color: $text-gray;
}

// svg rect
.bar {
  fill: $bar-fill;

  &:hover {
    fill: $mouseover;
  }
}

// axises
.tick {
  color: $text-gray;
}

.axis-label {
  font-size: 0.8rem;
}

.tooltip {
  position: absolute;
  width: 7rem;
  height: 3rem;
  font: 12px sans-serif;
  fill: $text-gray;
  text-align: center;
  background: $mouseover;
  border-style: none;
  border-radius: 15px;
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16), 0 3px 6px rgba(0, 0, 0, 0.23);
  align-items: center;
}
</style>
