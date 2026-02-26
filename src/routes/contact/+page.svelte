<svelte:head>
  <title>Contact - Summer Zhou</title>
</svelte:head>

<nav>
  <a href=".">Home</a>
  <a href="projects">Projects</a>
  <a class="current" href="contact">Contact</a>
  <a href="resume">Resume</a>
  <a href="https://github.com/SummerZ27" target="_blank" rel="noopener noreferrer">GitHub</a>
</nav>

<main>
  <h1>Contact</h1>

  <p>Feel free to reach out! You can use the form below to send me a message.</p>

  <form action="mailto:summerzhou233@gmail.com" method="GET">
    <label>
      <span>Email:</span>
      <input type="email" name="email" value="your.email@example.com">
    </label>

    <label>
      <span>Subject:</span>
      <input type="text" name="subject" value="Hello from your website">
    </label>

    <label>
      <span>Message:</span>
      <textarea name="body">Hi Summer, I'd like to get in touch...</textarea>
    </label>

    <button type="submit">Send</button>
  </form>
</main>
