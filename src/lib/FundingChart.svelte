<!-- src/lib/FundingChart.svelte -->
<script lang="ts">
  import { onMount } from "svelte";
  import * as d3 from "d3";

  // Define the type for your funding data
  type FundingData = {
    source: string;
    amount: number;
  };

  // Properly type your props
  let { data } = $props<{ data: FundingData[] }>();

  // Use HTMLDivElement instead of HTMLCanvasElement since we're using SVG
  let chartElement: HTMLDivElement;

  onMount(() => {
    const width = 200;
    const height = 200;
    const radius = Math.min(width, height) / 2;

    // Create SVG element
    const svg = d3
      .select(chartElement)
      .append("svg")
      .attr("width", width)
      .attr("height", height)
      .append("g")
      .attr("transform", `translate(${width / 2}, ${height / 2})`);

    // Create color scale
    const color = d3
      .scaleOrdinal<string>()
      .domain(data.map((d: FundingData) => d.source))
      .range(d3.schemeCategory10);

    // Create the pie layout
    const pieGenerator = d3.pie<FundingData>().value((d) => d.amount);

    // Generate the pie data
    const pieData = pieGenerator(data);

    // Create the arc generator
    const arcGenerator = d3
      .arc<d3.PieArcDatum<FundingData>>()
      .innerRadius(0)
      .outerRadius(radius);

    // Create the arcs
    const arcs = svg
      .selectAll(".arc")
      .data(pieData)
      .enter()
      .append("g")
      .attr("class", "arc");

    // Add paths to arcs
    arcs
      .append("path")
      .attr("d", (d) => arcGenerator(d) as string)
      .attr("fill", (d) => color(d.data.source));

    // Add labels
    arcs
      .append("text")
      .attr("transform", (d) => `translate(${arcGenerator.centroid(d)})`)
      .attr("text-anchor", "middle")
      .attr("font-size", "10px")
      .text((d) => d.data.source);
  });
</script>

<div bind:this={chartElement}></div>
