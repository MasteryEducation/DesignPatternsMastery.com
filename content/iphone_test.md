---
title: D3.js iPhone Mock
# draft: true
---

## Hello iPhone! 

{{< d3js >}} 

(function() {
    const width = 200;
    const height = 400;

    // Select the script's parent node and append a div using D3
    const container = d3.select(document.currentScript.parentNode)
        .append('div')
        .style('display', 'inline-block')
        .style('margin', '20px'); // Optional styling

    // Create an SVG container within the div
    const svg = container.append('svg')
        .attr('width', width)
        .attr('height', height);

    // Draw the iPhone body with rounded corners
    svg.append('rect')
        .attr('x', 0)
        .attr('y', 0)
        .attr('width', width)
        .attr('height', height)
        .attr('rx', 30) // Rounded corners
        .attr('ry', 30)
        .attr('fill', '#000'); // Black color for the iPhone body

    // Draw the screen area
    svg.append('rect')
        .attr('x', 10)
        .attr('y', 50)
        .attr('width', width - 20)
        .attr('height', height - 100)
        .attr('fill', '#fff'); // White color for the screen

    // Add the "Hello World" text to the screen
    svg.append('text')
        .attr('x', width / 2)
        .attr('y', height / 2)
        .attr('text-anchor', 'middle') // Center the text horizontally
        .attr('dominant-baseline', 'middle') // Center the text vertically
        .attr('font-size', '16px')
        .attr('fill', '#000')
        .text('Hello World');

    // Draw the home button (for older iPhone models)
    svg.append('circle')
        .attr('cx', width / 2)
        .attr('cy', height - 30)
        .attr('r', 10)
        .attr('fill', '#444');

    // Optional: Add speaker and camera notch
    svg.append('rect')
        .attr('x', width / 2 - 30)
        .attr('y', 15)
        .attr('width', 60)
        .attr('height', 5)
        .attr('fill', '#333');

    svg.append('circle')
        .attr('cx', width / 2)
        .attr('cy', 30)
        .attr('r', 3)
        .attr('fill', '#333');
})();

{{< /d3js >}}
