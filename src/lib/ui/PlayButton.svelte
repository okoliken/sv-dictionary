<script lang="ts">
  import { type Snippet } from "svelte";
  let {
    handleClick,
    children,
  }: {
    handleClick: () => void;
    children: Snippet;
  } = $props();
</script>

<button
  onclick={handleClick}
  class="w-[48px] lg:w-[75px] play-button h-[48px] lg:h-[75px] bg-[#e8d1fb] dark:bg-[#2d163f] flex items-center justify-center rounded-full duration-100 ease-in-out transition"
>
  {@render children()}
</button>

<style>
  :global(.play-button:hover svg) {
    color: #ffffff;
  }
  .play-button:hover {
    background: #a445ed;
  }
</style>
