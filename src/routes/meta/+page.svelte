<script>
  import { base } from '$app/paths';
  import { onMount } from 'svelte';
  import * as d3 from 'd3';
  import BarHorizontal from '$lib/BarHorizontal.svelte';

  let locData = [];
  let langData = [];

  onMount(async () => {
    locData = await d3.csv(`${base}/loc.csv`, row => ({
      ...row,
      line: Number(row.line),
      length: Number(row.length),
      depth: Number(row.depth)
    }));

    langData = d3.rollups(locData, v => v.length, d => d.type)
      .map(([lang, count]) => ({ label: lang, value: count }))
      .sort((a, b) => b.value - a.value);
  });
</script>

<svelte:head>
  <title>Meta - Summer Zhou</title>
</svelte:head>

<main>
  <h1>Meta</h1>
  <p>Stats and visualizations about the code behind this website.</p>

  <section class="chart-section">
    <h2>Lines of Code by Language</h2>
    {#if langData.length > 0}
      <BarHorizontal data={langData} />
    {:else}
      <p>Loading data...</p>
    {/if}
  </section>
</main>

<style>
  .chart-section {
    overflow: visible;
  }
</style>
