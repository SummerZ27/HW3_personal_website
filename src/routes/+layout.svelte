<script>
  import '../style.css';
  import { base } from '$app/paths';
  import { page } from '$app/stores';

  let root = globalThis.document?.documentElement;
  let localStorage = globalThis.localStorage ?? {};
  let colorScheme = localStorage.colorScheme ?? 'dark';

  let pages = [
    { url: '/', title: 'About' },
    { url: '/projects', title: 'Projects' },
    { url: '/resume', title: 'Resume' },
    { url: '/contact', title: 'Contact' },
    { url: '/meta', title: 'Meta' },
    { url: 'https://github.com/SummerZ27', title: 'GitHub' }
  ];

  $: root?.style.setProperty('color-scheme', colorScheme);
  $: root?.setAttribute('data-theme', colorScheme);
  $: localStorage.colorScheme = colorScheme;

  function toggleTheme() {
    colorScheme = colorScheme === 'dark' ? 'light' : 'dark';
  }
</script>

<button class="theme-toggle" type="button" on:click={toggleTheme}>
  {colorScheme === 'dark' ? 'Switch to light' : 'Switch to dark'}
</button>

<nav>
  {#each pages as p}
    <a
      href={p.url.startsWith('http') ? p.url : base + p.url}
      class:current={p.url === '/'
        ? $page.url.pathname === base + '/'
        : $page.url.pathname.startsWith(base + p.url)}
      target={p.url.startsWith('http') ? '_blank' : null}
      rel={p.url.startsWith('http') ? 'noopener noreferrer' : null}
    >
      {p.title}
    </a>
  {/each}
</nav>

<slot />

<style>
  .theme-toggle {
    position: absolute;
    top: 1rem;
    right: 1rem;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    border: 1px solid var(--border-color);
    background: var(--bg-card);
    color: var(--text-primary);
    padding: 0.5rem 0.75rem;
    border-radius: 999px;
    font-size: 0.8rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.2s ease;
    z-index: 101;
  }

  .theme-toggle:hover {
    border-color: var(--primary-color);
    transform: translateY(-1px);
  }

  .theme-toggle:focus-visible {
    outline: 2px solid var(--primary-color);
    outline-offset: 2px;
  }

  nav {
    background: var(--nav-bg, rgba(10, 10, 15, 0.8));
    backdrop-filter: blur(20px);
    border-bottom: 2px solid var(--nav-border-color, oklch(50% 10% 200 / 40%));
    padding: 1.5rem 2rem;
    margin-bottom: 0;
    position: sticky;
    top: 0;
    z-index: 100;
    box-shadow: 0 4px 30px rgba(0, 0, 0, 0.3);
    max-width: 100%;
  }

  nav a {
    display: inline-block;
    margin-right: 2rem;
    text-decoration: none;
    color: var(--text-secondary);
    font-weight: 500;
    font-size: 1rem;
    position: relative;
    transition: all 0.3s ease;
    padding: 0.5rem 0;
  }

  nav a::after {
    content: '';
    position: absolute;
    bottom: 0;
    left: 0;
    width: 0;
    height: 2px;
    background: linear-gradient(90deg, var(--primary-color), var(--accent-color));
    transition: all 0.3s ease;
  }

  nav a.current {
    color: var(--primary-color);
    font-weight: 600;
  }

  nav a.current::after {
    width: 100%;
    height: 4px;
    background: var(--nav-border-color, oklch(50% 10% 200 / 40%));
    box-shadow: 0 0 10px rgba(99, 102, 241, 0.5);
  }

  nav a:not(.current):hover {
    color: var(--text-primary);
    transform: translateY(-2px);
  }

  nav a:not(.current):hover::after {
    width: 100%;
    height: 3px;
    box-shadow: 0 0 8px rgba(99, 102, 241, 0.4);
  }

  nav a.current:hover {
    color: var(--accent-color);
    transform: translateY(-1px);
  }

  nav a.current:hover::after {
    height: 4px;
    box-shadow: 0 0 15px rgba(236, 72, 153, 0.6);
  }

  @media (max-width: 768px) {
    .theme-toggle {
      top: 0.75rem;
      right: 0.75rem;
    }

    nav {
      padding: 1rem;
      text-align: center;
    }

    nav a {
      display: block;
      margin: 0.5rem 0;
      margin-right: 0;
    }
  }
</style>
