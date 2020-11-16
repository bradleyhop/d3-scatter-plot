<script>
import * as d3 from 'd3';

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
    // fetch data provide by freeCodeCamp api
    fetch('https://raw.githubusercontent.com/freeCodeCamp/ProjectReferenceData/master/cyclist-data.json')
      .then((response) => response.json())
      .then((data) => {
        this.cycleData = data;
      })
      .then(() => this.graphInit())
      .catch((error) => console.log(error));
  },

  methods: {
    // called by mounted() after async data is fetch'ed
    graphInit() {
      console.log(this.cycleData);

      // choose element to put our svg element
      const svg = d3.select('#bar-chart')
        .append('svg')
        .attr('width', this.widthChart)
        .attr('height', this.heightChart);

      // setup x-axis (year)
      const xScale = d3.scaleLinear()
        // minus and plus one year to give padding to the circle
        .domain([
          d3.min(this.cycleData, (d) => d.Year - 1),
          d3.max(this.cycleData, (d) => d.Year + 1),
        ])
        .range([
          this.padding,
          this.widthChart - this.padding,
        ]);

      // setup x-axis (time in min:sec format)
      const yScale = d3.scaleLinear()
        .domain(
          Math.ceil(d3.min(this.cycleData, (d) => d.Seconds * 1000)),
          Math.ceil(d3.max(this.cycleData, (d) => d.Seconds * 1000)),
        )
        .range([
          this.heightChart - this.padding,
          this.padding,
        ]);

      // d3 methods for drawing x- & y-axis
      const xAxis = d3.axisBottom(xScale);
      const yAxis = d3.axisLeft(yScale)
        .tickFormat((d) => d3.timeFormat('%MS')(new Date(0).setSeconds(d.Seconds)));

      // draw x-axis
      svg.append('g')
        .attr('transform', `translate(0, ${this.heightChart - this.padding})`)
        .attr('id', 'x-axis') // id required for project
        .call(xAxis);

      // draw y-axis
      svg.append('g')
        .attr('transform', `translate(${this.padding}, 0)`)
        .attr('id', 'y-axis') // id required for project
        .call(yAxis);

      // draw data points as dots
      svg.selectAll('circle')
        .data(this.cycleData)
        .enter()
        .append('circle')
        .attr('cx', (d) => xScale(d.Year))
        .attr('cy', (d) => this.heightChart - yScale(d.Seconds))
        .attr('data-xvalue', (d) => d.Year) // int or Date
        .attr('data-yvalue', (d) => d3.timeFormat('%M:%S')(d.Seconds)) // use Date object
        .attr('r', 5)
        .attr('class', 'dot') // class required for project
        // change circle color based on whether doped or not; coordinate with legend
        .style('fill', (d) => (d.Doping !== '' ? 'green' : 'red'));

      // tooltips
      svg.selectAll('text')
        .data(this.cycleData)
        .enter()
        .append('text')
        .attr('x', (d) => xScale(d.Year) + 5)
        .attr('y', (d) => yScale(this.heightChart - yScale(d.Seconds)))
        .attr('id', 'tooltip') // id required for project
        .attr('class', 'tooltip')
        .text('butts');
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

// svg circle
/*
 * .dot {
 *   fill: #000;
 * }
 */

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
