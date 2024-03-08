<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';

    let data = [];

    let n_samples = 400;

    function calculateMLE(data) {
        let mean = d3.mean(data);
        const sd = d3.deviation(data);
        return {mean, sd};
    }

    function normalPDF(x, mean, sd) {
        let coefficient = 1 / (sd * Math.sqrt(2 * Math.PI));
        const exponent = -0.5 * Math.pow((x - mean) / sd, 2);
        return coefficient * Math.exp(exponent);
    }

    onMount(() => {
        d3.csv('samples_data.csv').then((dataset) => {
            data = dataset.map(d => d.Normal_Samples);
            let normalData = data.slice(0, n_samples);

            const width = 800;
            const height = 400;
            const margin = { top: 20, right: 20, bottom: 30, left: 20 };
            const svg = d3.select("#chart")
                .attr("width", width + margin.left + margin.right)
                .attr("height", height + margin.top + margin.bottom)
                .append("g")
                .attr("transform", "translate(" + margin.left + "," + margin.top + ")");


            const xScale = d3.scaleLinear()
                .domain([10, d3.max(normalData) + 40])
                .range([0, width]);
            const yScale = d3.scaleLinear()
                .domain([0, 0.15])
                .range([height, 0]);

            const xAxis = d3.axisBottom(xScale);
            const yAxis = d3.axisLeft(yScale);

            svg.append("g")
                .attr("transform", "translate(20," + height + ")")
                .call(xAxis);

            svg.append("g")
                .attr("transform", "translate(20,0)")
                .call(yAxis);


            const histogram = d3.histogram()
                .domain(xScale.domain())
                .thresholds(xScale.ticks(80));

            const bins = histogram(normalData);

            svg.selectAll("rect")
                .data(bins)
                .enter().append("rect")
                .attr("x", d => xScale(d.x0) + 1)
                .attr("width", d => Math.max(0, xScale(d.x1) - xScale(d.x0) - 1))
                .attr("y", d => yScale(d.length / normalData.length))
                .attr("height", d => height - yScale(d.length / normalData.length));
            
            const normalLine = d3.line()
                .x(d => xScale(d))
                .y(d => yScale(normalPDF(d, 50, 10)));


            const xValues = d3.range(0, d3.max(data), 0.5);
            console.log(normalLine(xValues))
            svg.append("path")
                .attr("class", "line")
                .attr("fill", "none")
                .attr("stroke", "steelblue")
                .attr("stroke-width", 5)
                .attr("d", normalLine(xValues));

            const mle = calculateMLE(normalData);
            const mleLine = d3.line()
                .x(d => xScale(d))
                .y(d => yScale(normalPDF(d, mle.mean, mle.sd)))

            svg.append("path")
                .datum(xValues)
                .attr("class", "line")
                .style("stroke", "red")
                .attr("fill", "none")

                .attr("stroke-width", 1.5)
                .attr("d", mleLine);
        })
    })
</script>

<div>
    <h1>Visualization</h1>
    <svg id="chart"></svg>
</div>