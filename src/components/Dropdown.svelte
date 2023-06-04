<script lang="ts">
  import { fade, scale } from "svelte/transition";
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();
  export let font: string;
</script>

<div
  in:scale
  out:fade
  class="bg-white dark:text-white dark:bg-[#1f1f1f] absolute w-full max-w-[150px]
  rounded-[16px] drop-down dark:shadow dark:shadow-[#a445ed] lg:max-w-[183px] top-10 z-50 right-20 p-4"
>
  <ul>
    {#each ["Lato", "Manrope", "Open Sans", "Playfair", "Roboto", "Palette Mosaic", "Poppins"] as fonts}
      <li
        class:text-[#a445ed]={font == fonts}
        class="mb-2 cursor-pointer p-px capitalize"
      >
        <button on:click={() => dispatch("switchFont", fonts)}>
          {fonts}
        </button>
      </li>
    {/each}
  </ul>
</div>

<style>
  .drop-down {
    box-shadow: 0px 5px 30px rgba(0, 0, 0, 0.1);
  }
</style>
