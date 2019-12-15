---
layout: post
title: Making React and D3 get along
tags: react javascript d3
---

D3.js is a powerful data visualization library, but it's not always the easiest to integrate with React. Both libraries handle changes to application state by making updates to the DOM behind the scenes, and when these node mutations are combined they can produce some unexpected side effects. However, by using only selected functions from D3 in our React projects we can achieve compelling results.

## What is D3.js?

D3 is a JavaScript library that can be used to manipulate web documents in a way that is much more powerful than with plain JavaScript DOM manipulation. It allows developers to display complex datasets in a variety of formats and to produce animations.

D3 employs a declarative approach that will seem familiar to React developers, but as the functions can operate on nodes before they have been created, its syntax can take some getting used to. For instance, here is an example where an arbitrary number of data points are injected into the text of an arbitrary number of `p` tags using the `enter` and `append` methods.

```js
d3.select("body")
  .selectAll("p")
  .data(anArrayofNumbers)
  .enter()
  .append("p")
  .text(data => `I’m number ${data}!`);
```

Another common pattern in D3 is to create transitions between changes to the data set. Here is an example where an svg `circle`'s radius is altered (based on a predefined `scale` function) after a staggered transition delay that also relies on the index of the data being passed in. With this code, the circles will expand or contract in a sequence.

```js
d3.selectAll("circle")
  .transition()
  .duration(750)
  .delay((data, index) => index * 10)
  .attr("r", data => Math.sqrt(data * scale));
```

These examples are just scratching the surface of what D3 is capable of. The D3 community has produced some incredible work, and I would recommend checking out some of the [cool projects](https://github.com/d3/d3/wiki/Gallery) that have been created with the tool if you're looking to dive deeper. Today I'm only going to touch on a few notable functions that we can use to help us generate a simple data visualization within our React application.

## Bar charts in React with D3

I recently completed a project which involved fetching data from the World Population API, and I needed to be able to compare anywhere from two to six different countries' populations in a dynamic bar graph. D3 is great for this use case, as there are built-in functions for generating a linear scale based on the data being passed in.

I began by defining a function to determine the largest value in my `populations` data set (which resides here in React's local component state), utilizing D3's `max` function.

```js
import * as d3 from "d3";
...
const highestVal = d3.max(props.populations, data => data.population);
```

I was then able to use this to create a `scale` function, which served to constrain and scale my bar graph based on the desired maximum height, using the built-in `scaleLinear`, `domain` and `range` functions.

```js
const MAX_HEIGHT = 500;
const PADDING = 30;
const scale = d3
  .scaleLinear()
  .domain([0, highestVal])
  .range([0, MAX_HEIGHT - PADDING]);
```

Using the same methodology as before, I created a `colour` function which applies a value from a range of hex codes for each bar within the graph, based on the value of its population data.

```js
const colour = d3
  .scaleLinear()
  .domain([0, highestVal])
  .range(["#c19898", "#4a4a48"]);
```

Here are the parent SVG `Chart` component and child `Bar` components, which will now be rendered with the desired parameters (note how the colour and scale functions are being used):

```jsx
<Chart width={dataLength * (BAR_WIDTH + BAR_MARGIN)} height={MAX_HEIGHT}>
  {props.populations.map((country, index) => {
    const itemHeight = country.population;
    return (
      <Bar
        key={country.name}
        country={country.name}
        population={country.population}
        color={colour(country.population)}
        x={index * (BAR_WIDTH + BAR_MARGIN) + X_OFFSET}
        y={MAX_HEIGHT - scale(itemHeight)}
        width={BAR_WIDTH}
        height={scale(itemHeight)}
      />
    );
  })}
</Chart>
```

And here is the code for the two rendered components. Note the use of the `svg` element's `transform` attribute, which allows for [much more precise and complex transformations](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/transform) than on HTML elements styled with CSS.

```jsx
import React from "react";
import "./App.css";

const Chart = ({ children, width, height }) => {
  return (
    <svg
      className="gridChart"
      viewBox={`0 0 ${width} ${height}`}
      width={width}
      height={height}
      display={"block"}
    >
      {children}
    </svg>
  );
};

export default Chart;
```

<br>

```jsx
import React from "react";
import "./App.css";

const Bar = ({ x, y, width, height, country, population, colour }) => {
  const TEXT_PADDING = 5;
  const NUM_PADDING = 5;
  let yVal = y + height + TEXT_PADDING;
  const xVal = x - TEXT_PADDING;
  const transform = `rotate(-90 ${xVal} ${yVal})`;
  return (
    <g>
      <text x={x} y={yVal} className="barText" transform={transform}>
        <tspan>{country}</tspan>
      </text>
      <text x={x} y={y - NUM_PADDING} className="barNums">
        {population.toLocaleString()}
      </text>
      <rect x={x} y={y} width={width} height={height} fill={colour} />
    </g>
  );
};

export default Bar;
```

This is just a simple example of how a few D3 functions can help you incorporate a bar chart in your React projects. The World Population API is no longer publicly available, so my project now uses hardcoded values for the population statistics. You can view an example of it [here](https://andrewnbishop.com/population-stats) and the full code is hosted [on GitHub](https://github.com/a-bishop/population-stats).
