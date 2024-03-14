<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';

    let full_data = {};
    let data = [];
    let n_samples = 400;
    let sampleType = 'Normal'; // Default sample type   
    let svg; // Declare svg here so it's accessible globally

    function calculateNormalMLE(data) {
        let mean = d3.mean(data);
        const sd = d3.deviation(data);
        return {mean, sd};
    }

    function normalPDF(x, mean, sd) {
        let coefficient = 1 / (sd * Math.sqrt(2 * Math.PI));
        const exponent = -0.5 * Math.pow((x - mean) / sd, 2);
        return coefficient * Math.exp(exponent);
    }


    // Calculate parameters for bimodal distribution
    const mean1 = 30;
    const sd1 = 8;
    const mean2 = 100;
    const sd2 = 10;
    const weight1 = 0.9;
    const weight2 = 0.1;

    function bimodalPDF(x, mean1, sd1, mean2, sd2, weight1, weight2) {
        const pdf1 = normalPDF(x, mean1, sd1);
        const pdf2 = normalPDF(x, mean2, sd2);
        return 2*(weight1 * pdf1 + weight2 * pdf2);
    }

    const lambda = 1/10;
    function exponentialPDF(x, lambda) {
        if (x < 0) return 0;
        return lambda * Math.exp(-lambda * x);
    }


    const width = 800;
    const height = 400;
    const margin = { top: 20, right: 20, bottom: 30, left: 20 };

    onMount(() => {
        svg = d3.select("#chart")
            .attr("width", width + margin.left + margin.right)
            .attr("height", height + margin.top + margin.bottom)
            .append("g")
            .attr("transform", "translate(" + margin.left + "," + margin.top + ")");
        
        loadData();
    })

    function loadData() {
        d3.csv('samples_data.csv').then((dataset) => {
            full_data = dataset
            updateChart();
        }).catch(error => {
            console.error('Error loading data:', error);
        });
    }
    function updateChart() {
        if (!svg) return; // Check if svg is properly initialized

        data = full_data.map(d => parseFloat(d[sampleType + '_Samples'])); // Dynamically select column based on sample type
        let normalData = data.slice(0, n_samples)
        console.log(d3.max(normalData))
        //console.log(normalData)
        const xScale = d3.scaleLinear()
            .domain([0, d3.max(normalData)])
            .range([0, width]);
        const yScale = d3.scaleLinear()
            .domain([0, 0.12])
            .range([height, 0]); 

        svg.selectAll("*").remove();

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
            .thresholds(xScale.ticks(70));

        const bins = histogram(normalData);

        svg.selectAll("rect")
            .data(bins)
            .enter().append("rect")
            .attr("fill", "green")
            .attr("x", d => xScale(d.x0) + 1)
            .attr("width", d => Math.max(0, xScale(d.x1) - xScale(d.x0) - 1))
            .attr("y", d => yScale(d.length / normalData.length))
            .attr("height", d => height - yScale(d.length / normalData.length));
        
        let lineFunction;
        
        // Determine which PDF function to use based on sampleType
        if (sampleType === 'Normal') {
            lineFunction = d3.line()
                .x(d => xScale(d))
                .y(d => yScale(normalPDF(d, 50, 10)));
        } else if (sampleType === 'Exponential') {
            lineFunction = d3.line()
                .x(d => xScale(d))
                .y(d => yScale(exponentialPDF(d, lambda)));
        } else if (sampleType === 'Bimodal') {
            // Calculate parameters for bimodal distribution
            lineFunction = d3.line()
                .x(d => xScale(d))
                .y(d => yScale(bimodalPDF(d, mean1, sd1, mean2, sd2, weight1, weight2)));
        }

        // Add path for PDF to the SVG
        const xValues = d3.range(0, d3.max(data), 0.5);
        svg.append("path")
            .attr("class", "line")
            .attr("fill", "none")
            .attr("stroke", "steelblue") // color for PDF
            .attr("stroke-width", 5)
            .attr("d", lineFunction(xValues));

        const mle = calculateNormalMLE(normalData);
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
             // Add legend
    const legend = d3.select("#legend");

    legend.selectAll("*").remove();

    legend.append("text")
        .attr("x", 10)
        .attr("y", 20)
        .style("fill", "steelblue")
        .text("True PMF Distribution");

    legend.append("text")
        .attr("x", 10)
        .attr("y", 40)
        .style("fill", "red")
        .text("Estimated Normal MLE");

    legend.append("text")
        .attr("x", 10)
        .attr("y", 60)
        .style("fill", "green")
        .text("Histogram");
    }

    $: {
        console.log(sampleType,n_samples);
        updateChart();
    }

    function estimateExponentialRate(data) {
        // Calculate the sample mean
        const mean = data.reduce((acc, val) => acc + val, 0) / data.length;
        
        // Estimate the rate parameter (lambda) as the reciprocal of the sample mean
        const lambda = 1 / mean;

        return lambda;
    }

</script>

<div>
    <h1>{sampleType} Visualization</h1>
    <input type="range" name="mySlider" id="mySlider" min="10" max="1000" bind:value={n_samples}>
    <select bind:value={sampleType}>
        <option value="Normal">Normal</option>
        <option value="Exponential">Exponential</option>
        <option value="Bimodal">Bimodal</option>
    </select>
    <svg id="chart"></svg>
    <svg id="legend" width="200" height="100">
        <!-- Legend content will be added here -->
    </svg>
</div>
<div>Number of Samples: {n_samples}</div>
