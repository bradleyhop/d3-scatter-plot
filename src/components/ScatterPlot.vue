<script>
import * as d3 from 'd3';

export default {
  name: 'ScatterPlot',

  data() {
    return {
      cycleData: undefined, // placeholder for fetch'ed gdp data
      widthChart: 1000, // width of svg area
      heightChart: 500, // height of svg area
      padding: 55, // padding of chart
    };
  },

  mounted() {
    // fetch data provide by freeCodeCamp api
    fetch('https://raw.githubusercontent.com/freeCodeCamp/ProjectReferenceData/master/cyclist-data.json')
      .then((response) => response.json())
      .then((data) => {
        // store obj within vue
        this.cycleData = data;

        // add DateObj key:property to represent Seconds as Date() object for d3 time formatting
        this.cycleData.forEach((obj) => (
          Object.assign(obj,
            {
              DateObj: new Date(obj.Seconds * 1000), // convert to milliseconds for Date()
            })
        ));
      })
      .then(() => this.graphInit())
      .catch((error) => console.log(error));
  },

  methods: {
    // called by mounted() after async data is successfully fetch'ed
    graphInit() {
      // choose element to put our svg element
      const svg = d3.select('#scatter-plot')
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

      // setup x-axis
      const yScale = d3.scaleTime()
        .domain(
          d3.extent(this.cycleData, (d) => d.DateObj),
        )
        .range([
          this.padding,
          this.heightChart - this.padding,
        ]);

      // functional declaration for drawing x-axis with no comma in the labels
      const xAxis = d3.axisBottom(xScale)
        .tickFormat(d3.format('d'));

      // functional declaration for drawing y-axis with label in minutes:seconds format
      const yAxis = d3.axisLeft(yScale)
        .tickFormat(d3.timeFormat('%M:%S'));

      // function declaration for tooltip div element
      const divTool = d3.select('#scatter-plot')
        .append('div')
        .attr('id', 'tooltip') // project requirement
        .style('opacity', 0);

      // draw x-axis
      svg.append('g')
        .attr('transform', `translate(0, ${this.heightChart - this.padding})`)
        .attr('id', 'x-axis') // project requirement
        .call(xAxis);

      // draw y-axis
      svg.append('g')
        .attr('transform', `translate(${this.padding}, 0)`)
        .attr('id', 'y-axis') // project requirement
        .call(yAxis);

      // draw data points as dots with tooltip popup on mouseover
      svg.selectAll('circle')
        .data(this.cycleData)
        .enter()
        .append('circle')
        .attr('cx', (d) => xScale(d.Year))
        .attr('cy', (d) => yScale(d.DateObj))
        .attr('data-xvalue', (d) => d.Year) // int or Date for project requirement
        .attr('data-yvalue', (d) => d.DateObj) // Date obj for project requirement
        .attr('r', 6)
        // class 'dot' project requirement
        // change circle color based on whether doped or not; coordinate with legend
        .attr('class', (d) => (d.Doping ? 'dot doped' : 'dot not-doped'))
        // hover to show value with tooltip as defined in divTool above
        .on('mouseover', (event, d) => {
          divTool
            .attr('data-year', d.Year) // project requirement
            .attr('class', d.Doping ? 'tooltip' : 'tooltip narrow')
            .html(`<p>
              <span class="name">${d.Name}</span>, ${d.Nationality}<br/>
              Year - ${d.Year}, Time - ${d.Time}<br/>
              <span class="doping-text">${d.Doping ? d.Doping : ''}</span>
            </p>`)
            .style('opacity', '1')
            .style('display', 'flex') // to align items centrally
            .style('top', `${event.pageY - 30}px`)
            .style('left', `${event.pageX + 12}px`);
        })
        .on('mouseout', () => {
          divTool
            .style('opacity', 0)
            .style('display', 'none');
        });
    },

  },
};
</script>

<template>
  <div class="container-scatter-plot">
    <h2 id="title" class="chart-title">
      Thirty-Five Fastest Times Up Alpe d'Huez
    </h2>
    <div id="scatter-plot" class="scatter-plot">
    </div>
  </div>
</template>

<style lang="scss">
/*
 * NB: Do not scope this component's style! D3 won't be able to see it!!!
 */

.container-scatter-plot {
  background-color: $chart-background;
  border-radius: 15px;
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16), 0 3px 6px rgba(0, 0, 0, 0.23);
  height: 550px;
  margin: auto;
  width: 1050px;
}

.chart-title {
  color: $text-gray;
  font-family: "Roboto", Helvetica, Arial, sans-serif;
  margin-bottom: 0;
  padding-top: 1rem;
}

// axises
.tick {
  color: $text-gray;
}

.axis-label {
  font-size: 0.8rem;
}

// dynanmically assigned class for dots to show doping allegation status
.doped {
  fill: $guilty-red;
}

// dynanmically assigned class for dots to show doping allegation status
.not-doped {
  fill: $clean-green;
}

.tooltip {
  align-items: center;
  background: $mouseover;
  border-radius: 15px;
  border-style: none;
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16), 0 3px 6px rgba(0, 0, 0, 0.23);
  color: #fff;
  font-family: Roboto, Helvetica, Arial, sans-serif;
  font-size: 13px;
  height: 5rem;
  justify-content: center;
  padding: 0 0.6rem 0 0.6rem;
  position: absolute;
  text-align: left;
  width: 14rem;

  & .name {
    font-weight: bold;
  }

  & .doping-text {
    font-style: italic;
  }
}

.narrow {
  @extend .tooltip;

  height: 3rem;
  width: 11rem;
}

</style>
