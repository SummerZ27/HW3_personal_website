<script>
  import { base } from '$app/paths';
  import projects from '$lib/projects.json';
  import Project from '$lib/Project.svelte';
  import reading from '$lib/reading.json';
  import ReadingItem from '$lib/ReadingItem.svelte';
</script>

<svelte:head>
  <title>Summer Zhou: Personal site and portfolio</title>
</svelte:head>

<nav>
  <a class="current" href=".">Home</a>
  <a href="projects">Projects</a>
  <a href="contact">Contact</a>
  <a href="resume">Resume</a>
  <a href="https://github.com/SummerZ27" target="_blank" rel="noopener noreferrer">GitHub</a>
</nav>

<main>
  <div class="header-section">
    <h1>Summer Zhou</h1>
    <img src="{base}/images/profile.png" alt="Portrait photo of Summer Zhou">
  </div>

  <p>Welcome to my personal website! I'm a researcher and engineer working at the intersection of AI/ML and systems engineering. My work spans agentic AI workflows, production systems optimization, bioinformatics applications, and advanced machine learning research.</p>

  <p>I'm passionate about building end-to-end systems that solve real-world problems—from AI-native content creation pipelines to manufacturing scheduling systems, from gene information platforms to hierarchical forecasting models. This site showcases my projects, research, and provides a way to get in touch.</p>

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
