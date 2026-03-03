<script>
  function handleSubmit(event) {
    event.preventDefault();
    let form = event.target;
    let data = new FormData(form);
    let params = [];

    for (let [name, value] of data) {
      params.push(`${encodeURIComponent(name)}=${encodeURIComponent(value)}`);
    }

    let url = `${form.action}?${params.join('&')}`;
    location.href = url;
  }
</script>

<svelte:head>
  <title>Contact - Summer Zhou</title>
</svelte:head>

<main>
  <h1>Contact</h1>

  <p>Feel free to reach out! You can use the form below to send me a message.</p>

  <form action="mailto:summerzhou233@gmail.com" method="GET" on:submit={handleSubmit}>
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
