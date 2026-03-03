<script>
  import projects from '$lib/projects.json';
  import Project from '$lib/Project.svelte';
</script>

<svelte:head>
  <title>Projects - Summer Zhou</title>
</svelte:head>

<main>
  <h1>Projects ({projects.length})</h1>

  <div class="projects">
    {#each projects as p}
      <Project data={p} />
    {/each}
  </div>
</main>
