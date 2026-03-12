<script>
  import { base } from '$app/paths';
  import Scrolly from 'svelte-scrolly';
  import projects from '$lib/projects.json';

  let scrollyProgress = 0;
  let sorted_projects = projects.sort((a, b) => a.year - b.year);
  let progressPerProject = 100 / sorted_projects.length;

  $: activeProjectIdx = Math.min(
    sorted_projects.length - 1,
    Math.floor(scrollyProgress / progressPerProject)
  );
</script>

<div class="scrolly-wrapper">
  <Scrolly bind:progress={scrollyProgress}>
    {#each sorted_projects as project}
      <section class="step">
        <div class="step-content">
          <h3>{project.title} <span class="year">({project.year})</span></h3>
          <p>{project.story}</p>
        </div>
      </section>
    {/each}

    <svelte:fragment slot="viz">
      <div class="project-detail">
        <h3>{sorted_projects[activeProjectIdx].year} — {sorted_projects[activeProjectIdx].title}</h3>
        <img
          src="{base}{sorted_projects[activeProjectIdx].image}"
          alt={sorted_projects[activeProjectIdx].title}
        />
      </div>
    </svelte:fragment>
  </Scrolly>
</div>

<style>
  .scrolly-wrapper {
    width: min(1000ch, 60vw);
    position: relative;
    left: 50%;
    transform: translateX(-50%);
  }

  .step {
    min-height: 80vh;
    padding: 2rem;
    display: flex;
    align-items: center;
  }

  .step-content {
    border-left: 3px solid var(--primary-color);
    padding: 1.5rem 2rem;
    background: var(--bg-card);
    border-radius: 0 12px 12px 0;
  }

  .step-content h3 {
    margin: 0 0 0.75rem 0;
    font-size: 1.3rem;
    color: var(--text-primary);
  }

  .step-content .year {
    color: var(--accent-color);
    font-weight: 500;
  }

  .step-content p {
    margin: 0;
    color: var(--text-secondary);
    line-height: 1.7;
    font-size: 0.95rem;
  }

  .project-detail {
    padding: 2rem;
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 60vh;
  }

  .project-detail h3 {
    margin: 0 0 1.5rem 0;
    font-size: 1.4rem;
    color: var(--text-primary);
    text-align: center;
    background: linear-gradient(135deg, var(--primary-color), var(--accent-color));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .project-detail img {
    width: 100%;
    max-width: 500px;
    height: auto;
    border-radius: 16px;
    border: 2px solid var(--border-color);
    box-shadow: var(--shadow-glow);
    transition: all 0.4s ease;
    animation: none;
  }

  .project-detail img:hover {
    transform: none;
    box-shadow: var(--shadow-glow);
  }

  @media (max-width: 768px) {
    .scrolly-wrapper {
      width: 100%;
      left: 0;
      transform: none;
    }

    .step {
      min-height: 60vh;
      padding: 1rem;
    }
  }
</style>
