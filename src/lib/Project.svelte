<script>
  import { base } from '$app/paths';
  export let data = {};
</script>

<section>
  <h2>{data.title}</h2>
  {#if data.image}
    <div class="project-content">
      <img src="{base}{data.image}" alt="{data.title}">
      <p>{data.description}</p>
    </div>
  {:else}
    <p>{data.description}</p>
  {/if}
</section>

<style>
  section {
    background: var(--bg-card);
    backdrop-filter: blur(10px);
    border: 1px solid var(--border-color);
    border-radius: 20px;
    padding: 2.5rem;
    margin-bottom: 2rem;
    transition: all 0.4s ease;
    position: relative;
    overflow: hidden;
    animation: fadeInUp 0.6s ease;
  }

  section::before {
    content: '';
    position: absolute;
    top: 0;
    left: -100%;
    width: 100%;
    height: 100%;
    background: linear-gradient(90deg, transparent, rgba(99, 102, 241, 0.1), transparent);
    transition: left 0.5s ease;
  }

  section:hover {
    transform: translateY(-5px);
    border-color: var(--primary-color);
    box-shadow: 0 10px 40px rgba(99, 102, 241, 0.2);
  }

  section:hover::before {
    left: 100%;
  }

  section h2 {
    margin-top: 0;
    margin-bottom: 0.5rem;
    font-size: 2rem;
    font-weight: 700;
    color: var(--text-primary);
    background: linear-gradient(135deg, var(--primary-color), var(--accent-color));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .project-content {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }

  section img {
    margin: 0;
    display: block;
    max-width: 100%;
    width: 100%;
    height: auto;
    border-radius: 20px;
    box-shadow: 0 0 20px rgba(99, 102, 241, 0.3);
    transition: all 0.4s ease;
    border: 2px solid var(--border-color);
  }

  section img:hover {
    transform: scale(1.05) rotate(2deg);
    box-shadow: 0 0 40px rgba(99, 102, 241, 0.5);
  }

  section p {
    margin: 0;
    color: var(--text-secondary);
  }

  @keyframes fadeInUp {
    from {
      opacity: 0;
      transform: translateY(30px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
</style>
