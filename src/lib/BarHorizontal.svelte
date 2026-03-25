<script>
  import * as d3 from 'd3';

  export let data = [];
  export let title = 'Lines of Code by Language';

  let width = 500;
  let height = 200;
  let margin = { top: 30, right: 100, bottom: 50, left: 80 };
  let innerWidth = width - margin.left - margin.right;
  let innerHeight = height - margin.top - margin.bottom;

  let xAxisEl, yAxisEl;

  $: xScale = d3.scaleLinear()
    .domain([0, d3.max(data, d => d.value) || 1])
    .range([0, innerWidth]);

  $: yScale = d3.scaleBand()
    .domain(data.map(d => d.label))
    .range([0, innerHeight])
    .padding(0.2);

  $: colorScale = d3.scaleOrdinal(d3.schemeTableau10)
    .domain(data.map(d => d.label));

  $: maxBar = data.length > 0 ? d3.greatest(data, d => d.value) : null;

  $: if (xAxisEl && yAxisEl) {
    d3.select(xAxisEl).call(
      d3.axisBottom(xScale)
        .ticks(Math.min(d3.max(data, d => d.value) || 1, 10))
        .tickFormat(d => Number.isInteger(d) ? d : '')
    );
    d3.select(yAxisEl).call(d3.axisLeft(yScale));
  }
</script>

<div class="container">
  <svg viewBox="0 0 {width} {height}">
    <g transform="translate({margin.left}, {margin.top})">
      <text
        x={innerWidth / 2}
        y={-margin.top / 2}
        text-anchor="middle"
        class="chart-title">
        {title}
      </text>

      {#each data as d}
        <rect
          x={0}
          y={yScale(d.label)}
          width={xScale(d.value)}
          height={yScale.bandwidth()}
          fill={colorScale(d.label)}
        />
      {/each}

      {#if maxBar && maxBar.value > 0}
        <rect
          x={0}
          y={yScale(maxBar.label)}
          width={xScale(maxBar.value)}
          height={yScale.bandwidth()}
          fill="none"
          stroke="currentColor"
          stroke-width="2"
        />
        <text
          x={xScale(maxBar.value) + 5}
          y={yScale(maxBar.label) + yScale.bandwidth() / 2}
          text-anchor="start"
          dominant-baseline="middle"
          class="annotation">
          {maxBar.label}: {maxBar.value}
        </text>
      {/if}

      <text
        x={innerWidth / 2}
        y={innerHeight + margin.bottom - 8}
        text-anchor="middle"
        class="axis-label">
        Lines of Code
      </text>

      <text
        x={-(innerHeight / 2)}
        y={-margin.left + 15}
        text-anchor="middle"
        transform="rotate(-90)"
        class="axis-label">
        Language
      </text>
    </g>

    <g transform="translate({margin.left}, {margin.top + innerHeight})"
       bind:this={xAxisEl} />
    <g transform="translate({margin.left}, {margin.top})"
       bind:this={yAxisEl} />
  </svg>
  <ul class="legend">
    {#each data as d}
      <li style="--color: {colorScale(d.label)}">
        <span class="swatch"></span>
        {d.label} <em>({d.value})</em>
      </li>
    {/each}
  </ul>
</div>

<style>
  svg {
    max-width: 100%;
    height: auto;
    overflow: visible;
    flex: 2;
    min-width: 0;
  }

  .container {
    display: flex;
    gap: 1.5rem;
    align-items: flex-start;
    overflow: visible;
  }

  .chart-title {
    font-size: 0.85em;
    font-weight: bold;
    fill: currentColor;
  }

  .axis-label {
    font-size: 0.7em;
    fill: currentColor;
  }

  .annotation {
    font-size: 0.6em;
    fill: currentColor;
    font-style: italic;
  }

  .legend {
    flex: 1;
    list-style: none;
    padding: 0;
    margin: 0;
    display: flex;
    flex-direction: column;
    gap: 0.4rem;
  }

  .legend li {
    display: flex;
    align-items: center;
    gap: 0.4rem;
    padding: 0;
    font-size: 0.8rem;
    color: var(--text-secondary);
  }

  .legend li::before {
    display: none;
  }

  .swatch {
    width: 12px;
    height: 12px;
    min-width: 12px;
    border-radius: 3px;
    background-color: var(--color);
  }

  .legend em {
    color: var(--text-secondary);
    font-style: normal;
    opacity: 0.7;
  }
</style>
