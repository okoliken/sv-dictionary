<script lang="ts">
  import { createEventDispatcher } from "svelte";
  const showSelectField = createEventDispatcher();
</script>

<button
  class="appearance-none outline-none p-0 m-0"
  on:click={() => showSelectField("selectFont")}
>
  <svg
    class="transform w-[25px] h-[25px] lg:-translate-x-5 text-[#a445ed]"
    xmlns="http://www.w3.org/2000/svg"
    width="1em"
    height="1em"
    viewBox="0 0 24 24"
    ><path
      fill="currentColor"
      d="M8.12 9.29L12 13.17l3.88-3.88a.996.996 0 1 1 1.41 1.41l-4.59 4.59a.996.996 0 0 1-1.41 0L6.7 10.7a.996.996 0 0 1 0-1.41c.39-.38 1.03-.39 1.42 0z"
    /></svg
  >
</button>
