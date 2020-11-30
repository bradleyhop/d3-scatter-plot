# d3-scatter-plot

Data Visualization Project for freeCodeCamp.org Data Visualization Project:
Scatter Plot Graph using Vue & D3.

## Project Info

See the [User
Stories](https://www.freecodecamp.org/learn/data-visualization/data-visualization-projects/visualize-data-with-a-scatterplot-graph)
for project requirements.

## Testing

freeCodeCamp offers a script that will test each User Story point. To test,
select the testing component, choose D3 Bar Chart from the dropdown menu, and
hit 'Run Tests'. Select the 'Tests' button to see details of test.

## Take-Aways

D3 can work with javascript `Date()` objects well, but it took me some time to
wrap my head around the D3's abstraction. The fetch'd data did not include a
`Date()` object directly, so I added a key: value to the local copy by
multiplying the given Seconds data by 1000 to create a new `Date()` object with
milliseconds.  From there, setting the scale with `d3.scaleTime()` is sufficient
for creating the y-axis and y-position of data plots; `d3.formatTime()` allowed
the ticks on the y-axis to have a MM:SS format.

Another hangup I had was not paying attention to method inputs.

```javascript
d3.domain(d3.extent()) // works because .extent() returns an array
d3.domain(d3.min(), d3.max()) // doesn't because min and max only returns values
d3.domain([d3.min(), d3.max()]) // works because now the returned values are in an array
```

This error only took me a couple of days to catch. Blarg. (I worked on other parts
while returning to this issue.)

Finally, the legend looks best superimposed on top of the graph, I think. To do
so, I first tried creating a new `<svg>` element. This only led to blockage and
positioning woes. Instead, the legend is now in a `<g>` element with
`d3.attr('transform', 'translate()')` to set the position within the graph's
`<svg>`. Setting the height, width, and contents is then trivial.
