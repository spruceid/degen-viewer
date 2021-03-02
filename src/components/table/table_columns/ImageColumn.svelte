<script lang="ts">
  export let props: Object;
  export let onclick: Function;
</script>

<div class="flex items-center">
  <!-- svelte-ignore a11y-missing-attribute -->
  <img on:click={() => onclick()} {...props} class="mr-4" />
  {#if props["text"]}
    {props["text"]}
  {/if}
</div>
