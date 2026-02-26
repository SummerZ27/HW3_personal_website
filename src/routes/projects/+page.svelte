<script>
  import projects from '$lib/projects.json';
  import Project from '$lib/Project.svelte';
</script>

<svelte:head>
  <title>Projects - Summer Zhou</title>
</svelte:head>

<nav>
  <a href=".">Home</a>
  <a class="current" href="projects">Projects</a>
  <a href="contact">Contact</a>
  <a href="resume">Resume</a>
  <a href="https://github.com/SummerZ27" target="_blank" rel="noopener noreferrer">GitHub</a>
</nav>

<main>
  <h1>Projects ({projects.length})</h1>

  <div class="projects">
    {#each projects as p}
      <Project data={p} />
    {/each}
  </div>
</main>
