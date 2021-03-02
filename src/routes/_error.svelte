<script lang="ts">
  export let status: string;
  export let error: Error;

  const dev = process.env.NODE_ENV === "development";
</script>

<svelte:head>
  <title>{status}</title>
</svelte:head>

<h1 class="text-white">{status}</h1>

<p class="text-white">{error.message}</p>

{#if dev && error.stack}
  <pre class="text-white">{error.stack}</pre>
{/if}

<style>
  h1,
  p {
    margin: 0 auto;
  }

  h1 {
    font-size: 2.8em;
    font-weight: 700;
    margin: 0 0 0.5em 0;
  }

  p {
    margin: 1em auto;
  }

  @media (min-width: 480px) {
    h1 {
      font-size: 4em;
    }
  }
</style>
