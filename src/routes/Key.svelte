<script lang="ts">
  import type { Snippet } from "svelte";
  import type { HTMLButtonAttributes } from "svelte/elements";

  type Position =
    "top" | "bottom" | "left" | "right" | "top-left" | "top-right" | "bottom-left" | "bottom-right";

  const {
    position,
    children,
    ...props
  }: HTMLButtonAttributes & { position?: Position; children: Snippet } = $props();

  const radius = "40px";
  const border = "1px solid #282826";
</script>

<button
  style:border-top-left-radius={position === "top-left" ? radius : 0}
  style:border-top-right-radius={position === "top-right" ? radius : 0}
  style:border-bottom-left-radius={position === "bottom-left" ? radius : 0}
  style:border-bottom-right-radius={position === "bottom-right" ? radius : 0}
  style:width="180px"
  style:height="90px"
  style:font-size="42pt"
  style:border-top={border}
  style:border-left={border}
  style:border-bottom={position?.includes("bottom") ? border : 0}
  style:border-right={position?.includes("right") ? border : 0}
  style:justify-content="center"
  style:align-items="center"
  style:transition-duration="0.3s"
  {...props}
>
  {@render children()}
</button>

<style>
  button:hover {
    background: #1111;
  }
</style>
