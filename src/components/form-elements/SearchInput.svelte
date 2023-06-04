<script lang="ts">
  import { scale } from "svelte/transition";
  import { quintOut } from "svelte/easing";
  import SearchIcon from "../form-elements/icons/SearchIcon.svelte";
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  let inputStyle =
    "bg-[#F4F4F4] rounded-[16px] dark:bg-[#1f1f1f] dark:text-white w-full max-w-[327px] min-h-[48px] max-h-[48px] sm:max-w-[736px] sm:min-h-[64px] sm:max-h-[64px] px-4 lg:px-6 outline-none appearance-none font-bold lg:text-[20px] text-[16px] relative placeholder:font-normal placeholder:text-[16px] focus-within:border border-[#a445ed]";

  let value = null;

  $: isFieldDirty = value === "" ? true : false;
</script>

<form
  on:submit|preventDefault={() => dispatch("searchWord", value)}
  class:error={isFieldDirty}
  class="relative max-w-[736px] w-full"
>
  <input
    class:error-bag={isFieldDirty}
    bind:value
    class={inputStyle}
    type="text"
    placeholder="Search for a word"
  />
  <SearchIcon />
</form>

{#if isFieldDirty}
  <p
    transition:scale={{ delay: 250, duration: 300, easing: quintOut }}
    class="text-[#FF5252] mt-2 text-[20px]"
  >
    Whoops, can’t be empty…
  </p>
{/if}

<style>
  :global(.error svg) {
    color: #ff5252;
  }
  .error-bag {
    border: 1px solid #ff5252;
    transition: all 0.2s ease;
  }
</style>
