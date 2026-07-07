<script lang="ts">
  import type { Snippet } from "svelte";
  import type { HTMLButtonAttributes } from "svelte/elements";

  const { children, ...props }: HTMLButtonAttributes & { children: Snippet } = $props();
</script>

<button
  style:width="180px"
  style:height="90px"
  style:font-size="42pt"
  style:border="2px solid white"
  style:justify-content="center"
  style:align-items="center"
  {...props}
>
  {@render children()}
</button>

<style>
  button:hover {
    background: #555;
  }
</style>
