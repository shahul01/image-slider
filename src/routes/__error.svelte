<script context="module">
  export const load = ({ error, status }) => {
    return {
      props: {
        error,
        status
      }
    }
  };
</script>

<script lang="ts">

  export let status;
	export let error;

  console.error(`status: `, status);
  console.error(`error: `, error);
</script>

<div class="error">

  <div class="title-container">
    {#if status === 404}
      <h1 class="err-title">Page Not Found</h1>
    {:else}
      <h1 class="err-title">Error</h1>
    {/if}

  </div>

  <h2 class="err-code">Error code: {status}</h2>

  <a href="/" sveltekit:prefetch class="error-redirect" >
    <button class="primary">
      Go to Home
    </button>
  </a>

  <div class="error-stack-trace">
    <p>Detailed error</p>

    {#if typeof error === 'object'}
      <p>{JSON.stringify(error)}</p>
    {:else}
      <p>{error}</p>
    {/if}

  </div>

</div>


<style lang="postcss">
  .error {
    @apply my-2 mx-6;
  }

  .title-container {
    @apply flex justify-center items-center;
    @apply my-6;
    @apply font-bold;
    @apply text-4xl;
  }

  h2 {
    @apply my-2;
  }

  .error-stack-trace {
    @apply my-4;
  }

</style>
