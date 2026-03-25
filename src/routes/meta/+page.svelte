<script>
  import { base } from '$app/paths';
  import { onMount } from 'svelte';
  import * as d3 from 'd3';
  import BarHorizontal from '$lib/BarHorizontal.svelte';
  import {
    computePosition,
    autoPlacement,
    offset,
  } from '@floating-ui/dom';

  let locData = [];
  let commits = [];
  let clickedCommits = [];

  let width = 1000, height = 600;
  let margin = { top: 10, right: 10, bottom: 30, left: 50 };
  let usableArea = {
    top: margin.top,
    right: width - margin.right,
    bottom: height - margin.bottom,
    left: margin.left
  };
  usableArea.width = usableArea.right - usableArea.left;
  usableArea.height = usableArea.bottom - usableArea.top;

  let xAxis, yAxis, yAxisGridlines;
  let hoveredIndex = -1;
  let commitTooltip;
  let tooltipPosition = { x: 0, y: 0 };

  $: hoveredCommit = commits[hoveredIndex] ?? hoveredCommit ?? {};

  $: [minDate, maxDate] = commits.length > 0
    ? d3.extent(commits.map(d => d.date))
    : [new Date(), new Date()];
  $: maxDatePlusOne = new Date(maxDate);
  $: maxDatePlusOne.setDate(maxDatePlusOne.getDate() + 1);

  $: xScale = d3.scaleTime()
    .domain([minDate, maxDatePlusOne])
    .range([usableArea.left, usableArea.right])
    .nice();

  $: yScale = d3.scaleLinear()
    .domain([24, 0])
    .range([usableArea.bottom, usableArea.top]);

  $: rScale = d3.scaleSqrt()
    .domain(d3.extent(commits, d => d.totalLines) || [0, 1])
    .range([5, 30]);

  $: {
    if (xAxis && yAxis) {
      d3.select(xAxis).call(d3.axisBottom(xScale));
      d3.select(yAxis).call(
        d3.axisLeft(yScale).tickFormat(d => String(d % 24).padStart(2, '0') + ':00')
      );
    }
    if (yAxisGridlines) {
      d3.select(yAxisGridlines).call(
        d3.axisLeft(yScale).tickFormat('').tickSize(-usableArea.width)
      );
    }
  }

  $: fileLengths = d3.rollups(locData, v => d3.max(v, r => r.line), d => d.file);
  $: averageFileLength = d3.mean(fileLengths, d => d[1]);
  $: workByPeriod = d3.rollups(locData, v => v.length, d => d.datetime?.toLocaleString('en', { dayPeriod: 'short' }));
  $: maxPeriod = d3.greatest(workByPeriod, d => d[1])?.[0];

  // Step 5: Filtered bar data
  $: selectedLines = clickedCommits.length > 0
    ? clickedCommits.flatMap(d => d.lines)
    : locData;
  $: selectedCounts = d3.rollup(selectedLines, v => v.length, d => d.type);
  $: allTypes = Array.from(new Set(locData.map(d => d.type)));
  $: barData = allTypes.map(type => ({
    label: String(type),
    value: selectedCounts.get(type) || 0
  }));
  $: barTitle = clickedCommits.length > 0
    ? `Language breakdown for ${clickedCommits.length} selected commit${clickedCommits.length > 1 ? 's' : ''}`
    : 'Language breakdown for entire website';

  onMount(async () => {
    locData = await d3.csv(`${base}/loc.csv`, row => ({
      ...row,
      line: Number(row.line),
      depth: Number(row.depth),
      length: Number(row.length),
      date: new Date(row.date + 'T00:00' + row.timezone),
      datetime: new Date(row.datetime)
    }));

    commits = d3.groups(locData, d => d.commit).map(([commit, lines]) => {
      let first = lines[0];
      let { author, date, time, timezone, datetime } = first;
      return {
        id: commit,
        url: 'https://github.com/SummerZ27/HW3_personal_website/commit/' + commit,
        author, date, time, timezone, datetime,
        hourFrac: datetime.getHours() + datetime.getMinutes() / 60,
        totalLines: lines.length,
        lines: lines
      };
    });

    commits = d3.sort(commits, d => -d.totalLines);
  });

  async function dotInteraction(index, evt) {
    let hoveredDot = evt.target;
    if (evt.type === 'mouseenter') {
      hoveredIndex = index;
      tooltipPosition = await computePosition(hoveredDot, commitTooltip, {
        strategy: 'fixed',
        middleware: [offset(5), autoPlacement()],
      });
    } else if (evt.type === 'mouseleave') {
      hoveredIndex = -1;
    } else if (evt.type === 'click') {
      let commit = commits[index];
      if (!clickedCommits.includes(commit)) {
        clickedCommits = [...clickedCommits, commit];
      } else {
        clickedCommits = clickedCommits.filter(c => c !== commit);
      }
    }
  }
</script>

<svelte:head>
  <title>Meta - Summer Zhou</title>
</svelte:head>

<main>
  <h1>Meta</h1>
  <p>Stats and visualizations about the code behind this website.</p>

  <dl class="stats">
    <dt>Total <abbr title="Lines of code">LOC</abbr></dt>
    <dd>{locData.length}</dd>
    <dt>Commits</dt>
    <dd>{commits.length}</dd>
    <dt>Files</dt>
    <dd>{d3.group(locData, d => d.file).size}</dd>
    <dt>Avg file length</dt>
    <dd>{averageFileLength ? Math.round(averageFileLength) : '...'} lines</dd>
    <dt>Most active period</dt>
    <dd>{maxPeriod ?? '...'}</dd>
  </dl>

  <h2>Commits by time of day</h2>

  <svg viewBox="0 0 {width} {height}">
    <g class="gridlines" transform="translate({usableArea.left}, 0)" bind:this={yAxisGridlines} />
    <g transform="translate(0, {usableArea.bottom})" bind:this={xAxis} />
    <g transform="translate({usableArea.left}, 0)" bind:this={yAxis} />
    <g class="dots">
      {#each commits as commit, index}
        <circle
          cx={xScale(commit.datetime)}
          cy={yScale(commit.hourFrac)}
          r={rScale(commit.totalLines)}
          fill="steelblue"
          fill-opacity="0.6"
          class:selected={clickedCommits.includes(commit)}
          on:mouseenter={evt => dotInteraction(index, evt)}
          on:mouseleave={evt => dotInteraction(index, evt)}
          on:click={evt => dotInteraction(index, evt)}
        />
      {/each}
    </g>
  </svg>

  <dl class="info tooltip"
      hidden={hoveredIndex === -1}
      bind:this={commitTooltip}
      style="top: {tooltipPosition.y}px; left: {tooltipPosition.x}px">
    <dt>Commit</dt>
    <dd><a href={hoveredCommit.url} target="_blank">{hoveredCommit.id}</a></dd>
    <dt>Date</dt>
    <dd>{hoveredCommit.datetime?.toLocaleString('en', { dateStyle: 'full' })}</dd>
    <dt>Time</dt>
    <dd>{hoveredCommit.datetime?.toLocaleString('en', { timeStyle: 'short' })}</dd>
    <dt>Author</dt>
    <dd>{hoveredCommit.author}</dd>
    <dt>Lines edited</dt>
    <dd>{hoveredCommit.totalLines}</dd>
  </dl>

  <section class="chart-section">
    <h2>Language breakdown</h2>
    {#if barData.length > 0}
      <BarHorizontal data={barData} title={barTitle} />
    {:else}
      <p>Loading data...</p>
    {/if}
  </section>
</main>

<style>
  svg {
    overflow: visible;
    max-width: 100%;
    height: auto;
  }

  .gridlines {
    stroke-opacity: 0.2;
  }

  circle {
    cursor: pointer;
    transition: 200ms;
  }

  circle:hover {
    fill: darkgreen;
  }

  .selected {
    fill: #f59e0b;
  }

  .stats {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    gap: 0;
    text-align: center;
    margin: 1.5rem 0;
    padding: 1.5rem;
    background: var(--bg-card);
    border: 1px solid var(--border-color);
    border-radius: 16px;
  }

  .stats dt {
    grid-row: 1;
    font-size: 0.8rem;
    font-weight: 600;
    color: var(--text-secondary);
    text-transform: uppercase;
    letter-spacing: 0.05em;
    padding: 0.5rem;
  }

  .stats dd {
    grid-row: 2;
    font-size: 1.6rem;
    font-weight: 800;
    margin: 0;
    padding: 0.5rem;
    background: linear-gradient(135deg, var(--primary-color), var(--accent-color));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  dl.info {
    display: grid;
    grid-template-columns: auto 1fr;
    gap: 0.25rem 1rem;
    background: oklch(15% 2% 200 / 90%);
    backdrop-filter: blur(12px);
    padding: 1rem 1.25rem;
    border-radius: 12px;
    border: 1px solid var(--border-color);
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.4);
    font-size: 0.85rem;
    transition-duration: 500ms;
    transition-property: opacity, visibility;
  }

  dl.info[hidden]:not(:hover, :focus-within) {
    opacity: 0;
    visibility: hidden;
  }

  dl.info dt {
    font-weight: 600;
    color: #a1a1aa;
    text-transform: uppercase;
    font-size: 0.7rem;
    letter-spacing: 0.05em;
  }

  dl.info dd {
    margin: 0;
    color: #ffffff;
  }

  dl.info dd a {
    color: #818cf8;
    text-decoration: none;
    font-family: monospace;
    font-size: 0.8rem;
  }

  .tooltip {
    position: fixed;
    z-index: 200;
    pointer-events: none;
  }

  .chart-section {
    overflow: visible;
    margin-top: 2rem;
  }
</style>
