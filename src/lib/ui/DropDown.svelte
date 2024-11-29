<script lang="ts">
  import { fade, scale } from "svelte/transition";

  type Props = {
    showSelect: boolean;
    font: string;
    switchFont: (font: string) => void;
  }

  let {
    showSelect = $bindable(false),
    font,
    switchFont,
  }: Props = $props();

  let dropdownEl: HTMLDivElement | null = $state(null);

  const AVAILABLE_FONTS = [
    'Lato',
    'Manrope',
    'Open Sans',
    'Playfair',
    'Roboto',
    'Palette Mosaic',
    'Poppins'
  ] as const;

  const handleClick = (e: MouseEvent) => {
    if (dropdownEl && !dropdownEl.contains(e.target as Node)) {
      showSelect = false;
    }
  }

  // Transition parameters
  const scaleParams = { duration: 200, start: 0.95 };
  const fadeParams = { duration: 150 };
</script>

<svelte:window onclick={handleClick} />

{#if showSelect}
  <div
    bind:this={dropdownEl}
    in:scale={scaleParams}
    out:fade={fadeParams}
    class="dropdown bg-white dark:text-white dark:bg-[#1f1f1f] absolute w-full max-w-[150px]
      rounded-2xl dark:shadow dark:shadow-[#a445ed] lg:max-w-[183px] top-10 z-50 right-20 p-4"
  >
    <ul>
      {#each AVAILABLE_FONTS as fonts}
        <li class="font-option" class:selected={font === fonts}>
          <button onclick={() => switchFont(fonts)}>
            {fonts}
          </button>
        </li>
      {/each}
    </ul>
  </div>
{/if}

<style lang="postcss">
  .dropdown { 
    box-shadow: 0px 5px 30px rgba(0, 0, 0, 0.1);
  }

  .font-option {
    @apply mb-2 cursor-pointer p-px capitalize;
  }

  .font-option.selected {
    @apply text-[#a445ed];
  }

  .font-option button {
    @apply w-full text-left text-sm lg:text-base;
  }
</style>
