<script>
  import { base } from '$app/paths';
  export let data = {};
  const imageSrc = data.image?.startsWith('http') ? data.image : `${base}${data.image || ''}`;
</script>

<div class="reading-item">
  {#if data.image}
    <img src={imageSrc} alt="{data.title}">
  {/if}
  <div class="reading-details">
    <h3>{data.title}</h3>
    <p class="author">{data.author}</p>
    {#if data.description}
      <p class="description">{data.description}</p>
    {/if}
    {#if data.link}
      <a href={data.link} target="_blank" rel="noopener noreferrer" class="reading-link">Read more</a>
    {/if}
  </div>
</div>

<style>
  .reading-item {
    display: flex;
    gap: 1.5rem;
    align-items: flex-start;
    padding: 1.25rem;
    background: var(--bg-card);
    border: 1px solid var(--border-color);
    border-radius: 16px;
    margin-bottom: 1rem;
    transition: all 0.3s ease;
  }

  .reading-item:hover {
    border-color: var(--primary-color);
    box-shadow: 0 4px 20px rgba(99, 102, 241, 0.15);
  }

  .reading-item img {
    width: 80px;
    min-width: 80px;
    height: auto;
    border-radius: 8px;
    border: 1px solid var(--border-color);
    object-fit: cover;
  }

  .reading-details {
    flex: 1;
    min-width: 0;
  }

  .reading-details h3 {
    font-size: 1.25rem;
    font-weight: 600;
    color: var(--text-primary);
    margin: 0 0 0.25rem 0;
  }

  .author {
    font-size: 0.95rem;
    color: var(--text-secondary);
    margin: 0 0 0.5rem 0;
    font-style: italic;
  }

  .description {
    font-size: 0.9rem;
    color: var(--text-secondary);
    margin: 0 0 0.5rem 0;
    line-height: 1.5;
  }

  .reading-link {
    font-size: 0.9rem;
    color: var(--primary-color);
    text-decoration: none;
    font-weight: 500;
    transition: color 0.2s ease;
  }

  .reading-link:hover {
    color: var(--accent-color);
  }
</style>
