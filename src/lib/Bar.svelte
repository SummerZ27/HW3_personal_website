<script>
  import * as d3 from 'd3';

  let width = 400;
  let height = 300;
  let margin = { top: 40, right: 150, bottom: 80, left: 60 };
  let innerWidth = width - margin.left - margin.right;
  let innerHeight = height - margin.top - margin.bottom;

  export let data = [];

  let selectedIndex = -1;
  let liveText = '';
  let showChart = true;

  $: xScale = d3.scaleBand()
    .domain(data.map(d => d.label))
    .range([0, innerWidth])
    .padding(0.2);

  $: yScale = d3.scaleLinear()
    .domain([0, d3.max(data, d => d.value) || 1])
    .range([innerHeight, 0]);

  $: colorScale = d3.scaleOrdinal()
    .domain(data.map(d => d.label))
    .range(d3.quantize(d3.interpolateBlues, Math.max(data.length, 1)));

  $: maxBar = data.length > 0 ? d3.greatest(data, d => d.value) : null;

  $: description = data.length > 0
    ? `A bar chart showing project counts by year. ${data.map(d => `${d.label}: ${d.value} projects`).join(', ')}.`
    : 'A bar chart showing project counts by year. No data yet.';

  let xAxis, yAxis;

  $: if (xAxis && yAxis) {
    d3.select(xAxis).call(d3.axisBottom(xScale));
    d3.select(yAxis).call(
      d3.axisLeft(yScale)
        .tickFormat(d => Number.isInteger(d) ? d : '')
        .tickValues(d3.range(0, (d3.max(data, d => d.value) || 0) + 1))
    );
  }

  function announceBar(index) {
    const d = data[index];
    if (d) liveText = `${d.label}: ${d.value} projects selected.`;
  }

  function toggleBar(index, event) {
    if (event.type === 'keyup' && event.key && event.key !== 'Enter' && event.key !== ' ') return;
    selectedIndex = index;
    announceBar(index);
  }

  function toggleView() {
    showChart = !showChart;
    liveText = showChart ? 'Bar chart view shown.' : 'Table view shown.';
  }

  function barKeydown(index, event) {
    if (event.key === ' ') event.preventDefault();
  }
</script>

<button
  type="button"
  on:click={toggleView}
  aria-pressed={!showChart}
  aria-label="Toggle between bar chart and table view"
  class="toggle-button">
  {showChart ? 'Show table' : 'Show chart'}
</button>

{#if showChart}
  <div class="container">
    <svg
      viewBox="0 0 {width} {height}"
      role="img"
      aria-labelledby="bar-title bar-desc">
      <title id="bar-title">Projects per year</title>
      <desc id="bar-desc">{description}</desc>

      <g transform="translate({margin.left}, {margin.top})">
        <text
          x={innerWidth / 2}
          y={-margin.top / 2}
          text-anchor="middle"
          class="chart-title">
          Projects per Year
        </text>

        {#each data as d, index}
          <rect
            class="bar-rect"
            x={xScale(d.label)}
            y={yScale(d.value)}
            width={xScale.bandwidth()}
            height={innerHeight - yScale(d.value)}
            fill={colorScale(d.label)}
            opacity={selectedIndex === -1 || selectedIndex === index ? 1 : 0.45}
            tabindex="0"
            role="button"
            aria-label={`Year ${d.label}, ${d.value} projects. Select for details.`}
            on:click={e => toggleBar(index, e)}
            on:keyup={e => toggleBar(index, e)}
            on:keydown={e => barKeydown(index, e)}
          />
        {/each}

        {#if maxBar}
          <rect
            class="annotation-outline"
            x={xScale(maxBar.label)}
            y={yScale(maxBar.value)}
            width={xScale.bandwidth()}
            height={innerHeight - yScale(maxBar.value)}
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            tabindex="-1"
            aria-hidden="true"
            pointer-events="none"
          />
          <line
            x1={xScale(maxBar.label) + xScale.bandwidth()}
            y1={yScale(maxBar.value) + (innerHeight - yScale(maxBar.value)) / 2}
            x2={xScale(maxBar.label) + xScale.bandwidth() + 30}
            y2={yScale(maxBar.value) + (innerHeight - yScale(maxBar.value)) / 2}
            stroke="currentColor"
            stroke-width="1"
            aria-hidden="true"
            pointer-events="none"
          />
          <text
            x={xScale(maxBar.label) + xScale.bandwidth() + 35}
            y={yScale(maxBar.value) + (innerHeight - yScale(maxBar.value)) / 2}
            dominant-baseline="middle"
            class="annotation"
            pointer-events="none">
            Year with most projects
          </text>
        {/if}

        <text
          x={innerWidth / 2}
          y={innerHeight + margin.bottom - 10}
          text-anchor="middle"
          class="axis-label">
          Year
        </text>

        <text
          x={-(innerHeight / 2)}
          y={-margin.left + 15}
          text-anchor="middle"
          transform="rotate(-90)"
          class="axis-label">
          Number of Projects
        </text>
      </g>

      <g transform="translate({margin.left}, {margin.top + innerHeight})"
         bind:this={xAxis} />
      <g transform="translate({margin.left}, {margin.top})"
         bind:this={yAxis} />
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
{:else}
  <table aria-label="Table showing project counts by year" class="data-table">
    <caption>Projects by year</caption>
    <thead>
      <tr>
        <th id="year-header" scope="col">Year</th>
        <th id="projects-header" scope="col">Projects</th>
      </tr>
    </thead>
    <tbody>
      {#each data as d, i}
        <tr>
          <th id="row-{i}" scope="row">{d.label}</th>
          <td aria-labelledby="row-{i} projects-header">{d.value}</td>
        </tr>
      {/each}
    </tbody>
  </table>
{/if}

<p aria-live="polite" class="sr-only">{liveText}</p>

<style>
  .toggle-button {
    display: inline-flex;
    align-items: center;
    margin-bottom: 1rem;
    padding: 0.5rem 1rem;
    font-size: 0.9rem;
    font-weight: 600;
    color: var(--text-primary);
    background: var(--bg-card);
    border: 1px solid var(--border-color);
    border-radius: 8px;
    cursor: pointer;
    transition: border-color 0.2s ease, box-shadow 0.2s ease;
  }

  .toggle-button:hover {
    border-color: var(--primary-color);
  }

  .toggle-button:focus-visible {
    outline: 2px solid var(--primary-color);
    outline-offset: 2px;
  }

  svg {
    max-width: 100%;
    height: auto;
    overflow: visible;
    flex: 2;
    min-width: 0;
  }

  .container {
    display: flex;
    gap: 2rem;
    align-items: flex-start;
    overflow: visible;
  }

  .bar-rect {
    transition: opacity 300ms;
    outline: none;
    stroke: #0a0a0a;
    stroke-width: 1;
    cursor: pointer;
  }

  .bar-rect:focus-visible {
    stroke: #ffffff;
    stroke-width: 2px;
    stroke-dasharray: 4;
  }

  .chart-title {
    font-size: 1em;
    font-weight: bold;
    fill: currentColor;
  }

  .axis-label {
    font-size: 0.8em;
    fill: currentColor;
  }

  .annotation {
    font-size: 0.7em;
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
    gap: 0.5rem;
  }

  .legend li {
    display: flex;
    align-items: center;
    gap: 0.4rem;
    padding: 0;
    font-size: 0.9rem;
    color: var(--text-secondary);
  }

  .legend li::before {
    display: none;
  }

  .swatch {
    width: 14px;
    height: 14px;
    min-width: 14px;
    border-radius: 3px;
    background-color: var(--color);
    border: 1px solid rgba(0, 0, 0, 0.35);
  }

  .legend em {
    color: var(--text-secondary);
    font-style: normal;
    opacity: 0.7;
  }

  .data-table {
    margin-top: 0;
    margin-bottom: 1rem;
    border-collapse: collapse;
    width: 100%;
    max-width: 30em;
  }

  .data-table caption {
    font-weight: bold;
    margin-bottom: 0.5em;
    text-align: left;
    color: var(--text-primary);
  }

  .data-table th,
  .data-table td {
    border: 1px solid var(--border-color);
    padding: 0.5em;
    text-align: left;
    color: var(--text-primary);
  }

  .data-table thead th {
    background: var(--bg-card);
    color: var(--text-secondary);
    font-size: 0.85rem;
    text-transform: uppercase;
    letter-spacing: 0.04em;
  }

  .data-table tbody th {
    font-weight: 600;
  }

  .sr-only {
    position: absolute;
    left: -9999px;
    width: 1px;
    height: 1px;
    overflow: hidden;
  }
</style>
