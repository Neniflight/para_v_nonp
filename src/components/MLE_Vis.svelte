<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';

    let points = [];
    let meanValue = 30; // Initial mean value for Gaussian distribution
    let varianceValue = 5; // Initial variance value for Gaussian distribution
    let likelihoodValue = -200; // Initialize likelihood value
    let maxLikelihood = -200; // Initialize maximum likelihood estimate
    let bestMean
    let bestVariance
    let svg; 
    var margin = {top: 10, right: 30, bottom: 30, left: 60},
    width = 600 - margin.left - margin.right,
    height = 400 - margin.top - margin.bottom;

    function normalPDF(x, mean, sd) {
        let coefficient = 1 / (sd * Math.sqrt(2 * Math.PI));
        const exponent = -0.5 * Math.pow((x - mean) / sd, 2);
        return coefficient * Math.exp(exponent);
    }

    function likelihood(some_points) {
        if (some_points.length === 0) {
            return NaN;
        }
        let likelihood = some_points.map(point => normalPDF(point, meanValue, varianceValue));
        return Math.log(likelihood.reduce((acc, curr) => acc * curr, 1)) + 200; 
    }

    function updateLikelihood() {
        likelihoodValue = likelihood(points);
        if (likelihoodValue > maxLikelihood) {
            maxLikelihood = likelihoodValue;
            bestMean = meanValue;
            bestVariance = varianceValue;
        }
    }


    onMount(() => {
        d3.csv('samples_data.csv').then(data => {
        points = data.map(d => +d['Normal_Samples']).slice(0, 25);

        svg = d3.select('#mle_vis')
            .append('svg')
                .attr("width", width + margin.left + margin.right)
                .attr("height", height + margin.top + margin.bottom)
            .append("g")
                .attr("transform",
                    "translate(" + margin.left + "," + margin.top + ")");
        updateLikelihood();
        renderGraph();
        });
    });


    function renderGraph() {
        if (!svg) return; // Check if svg is properly initialized

        svg.selectAll('*').remove(); // Clear previous graph

        const xScale = d3.scaleLinear()
            .domain([20, 80])
            .range([0, width]);
        svg.append("g")
            .attr("transform", "translate(0," + height + ")")
            .call(d3.axisBottom(xScale))

        const yScale = d3.scaleLinear()
            .domain([0, 0.16]) // Probability range
            .range([height, 0]);
        svg.append("g")
            .call(d3.axisLeft(yScale));


        svg.append('g')
            .selectAll("dot")
            .data(points)
            .enter()
            .append("circle")
            .attr("cx", function (d) { return xScale(d); } )
            .attr("cy", function (d) { return yScale(0);} )
            .attr("r", 4)
            .style("fill", "#69b3a2")
        
        const xValues = d3.range(20, d3.max(points) + 5, 0.5);

        const mleLine = d3.line()
            .x(d => xScale(d))
            .y(d => yScale(normalPDF(d, meanValue, varianceValue)))
        
        svg.append("path")
            .datum(xValues)
            .attr("class", "line")
            .style("stroke", "red")
            .attr("fill", "none")
            .attr("stroke-width", 1.5)
            .attr("d", mleLine);
    }

    $: {
        console.log(meanValue,varianceValue);
        updateLikelihood();
        renderGraph();
    }
  </script>


<div class="vis-container">
    <div>
        <div>
            <h3>Mean: {meanValue}</h3>
            <input id="myMeanSlider" type="range" bind:value={meanValue}  min={25} max={70} step={1} />
            <h3>Variance: {varianceValue}</h3>
            <input id="myVarSlider" type="range" bind:value={varianceValue} min={1} max={20} step={0.2} />
        </div>
    
    
        <div id="mle_vis"></div>
    </div>

    <div class="stats-container">
        <h4>Best Mean So Far: {bestMean}</h4>
        <h4>Best Standard Deviation So Far: {bestVariance}</h4>
        <p>Likelihood of the Points: <b>{likelihoodValue.toFixed(2)}</b></p>
        <p>Maximum Likelihood: <b>{maxLikelihood.toFixed(2)}</b></p>
        <p>*Note that the likelihood is logged and scaled for cleanliness*</p>
    </div>

</div>

<style>
    h3,  h4, input, p {
        font-family: 'Verdana'
    }

    .vis-container {
        display: flex;
        width:100%;
    }

    .stats-container {
        background-color: white;
        padding: 0.3rem;
        height: 100%;
    }
</style>