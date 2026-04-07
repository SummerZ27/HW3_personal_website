<script>
  import { base } from '$app/paths';
  import { onMount } from 'svelte';
  import projects from '$lib/projects.json';
  import Project from '$lib/Project.svelte';
  import reading from '$lib/reading.json';
  import ReadingItem from '$lib/ReadingItem.svelte';

  let githubData = null;
  let loading = true;
  let error = null;

  onMount(async () => {
    try {
      let response = await fetch('https://api.github.com/users/SummerZ27');
      githubData = await response.json();
    } catch (err) {
      error = err;
    }
    loading = false;
  });
</script>

<svelte:head>
  <title>Summer Zhou: Personal site and portfolio</title>
</svelte:head>

<main>
  <div class="header-section">
    <h1>Summer Zhou</h1>
    <img src="{base}/images/profile.png" alt="Portrait of Summer Zhou">
  </div>

  <p>Welcome to my personal website! I'm a researcher and engineer working at the intersection of AI/ML and systems engineering. My work spans agentic AI workflows, production systems optimization, bioinformatics applications, and advanced machine learning research.</p>

  <p>I'm passionate about building end-to-end systems that solve real-world problems—from AI-native content creation pipelines to manufacturing scheduling systems, from gene information platforms to hierarchical forecasting models. This site showcases my projects, research, and provides a way to get in touch.</p>

  {#if loading}
    <p>Loading GitHub stats...</p>
  {:else if error}
    <p>Something went wrong: {error.message}</p>
  {:else}
    <section class="github-stats">
      <h2>My GitHub Stats</h2>
      <dl>
        <dt>Public Repos</dt>
        <dd>{githubData.public_repos}</dd>
        <dt>Followers</dt>
        <dd>{githubData.followers}</dd>
        <dt>Following</dt>
        <dd>{githubData.following}</dd>
        <dt>Public Gists</dt>
        <dd>{githubData.public_gists}</dd>
      </dl>
    </section>
  {/if}

  <h2>Latest projects</h2>
  <div class="projects highlights">
    {#each projects.slice(0, 3) as p}
      <Project data={p} />
    {/each}
  </div>

  <h2>What I'm Reading</h2>
  <div class="reading">
    {#each reading as item}
      <ReadingItem data={item} />
    {/each}
  </div>
</main>

<style>
  .github-stats dl {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 0;
    text-align: center;
    margin: 0;
  }

  .github-stats dt {
    grid-row: 1;
    font-size: 0.9rem;
    font-weight: 600;
    color: var(--text-secondary);
    text-transform: uppercase;
    letter-spacing: 0.05em;
    padding: 0.5rem;
  }

  .github-stats dd {
    grid-row: 2;
    font-size: 2rem;
    font-weight: 800;
    margin: 0;
    padding: 0.5rem;
    background: linear-gradient(135deg, var(--primary-color), var(--accent-color));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }
</style>
