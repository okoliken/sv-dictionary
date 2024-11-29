<script lang="ts">
  import Input from "./lib/ui/Input.svelte";
  import { onMount } from "svelte";
  import { fade, slide } from 'svelte/transition';
  import ThemeIcon from "./lib/icons/ThemeIcon.svelte";
  import BookLogo from "./lib/icons/BookLogo.svelte";
  import ThemeToggle from "./lib/ui/ThemeToggle.svelte";
  import IconEmogi from "./lib/icons/IconEmogi.svelte";
  import IconArrow from "./lib/icons/IconArrow.svelte";
  import Dropdown from "./lib/ui/DropDown.svelte";
  import Skeleton from "./lib/Skeleton.svelte";
  import Meanings from "./lib/ui/WordDefinition.svelte";
  import type { DictionaryResponse } from "./utils/types";

  let showSelect = $state(false);
  let font = $state("Poppins");
  let searching = $state(false);
  let meaning: DictionaryResponse[] = $state([]);
  let darkMode = $state(false);
  let error = $state<{
    title: string;
    message: string;
    resolution: string;
  } | null>(null);

  const selectedFont = $derived(font.replace("-", " ") || font);

  const searchForWords = async (word: string) => {
    searching = true;
    error = null;
    try {
      const res = await fetch(
        `https://api.dictionaryapi.dev/api/v2/entries/en/${word}`
      );
      const data = await res.json();

      if (res.ok) {
        meaning = [data[0]];
      } else {
        error = {
          title: data.title,
          message: data.message,
          resolution: data.resolution,
        }; 
      }
    } catch (error) {
      error = {
        title: "Error",
        message: "An error occurred while fetching the definition.",
        resolution: "Please try again later.",
      };
    } finally {
      searching = false;
    }
  };

  const changeTheme = (isToggled: boolean) => {
    localStorage.setItem("isToggled", JSON.stringify(isToggled));
    darkMode = isToggled;
  };

  onMount(() => {
    const storedTheme = localStorage.getItem("isToggled");
    darkMode = storedTheme ? JSON.parse(storedTheme) : false;

    if (darkMode) {
      document.documentElement.classList.add("dark");
    } else {
      document.documentElement.classList.remove("dark");
      localStorage.setItem("isToggled", "false");
    }
  });

  const handleFontSwitch = (newFont: string) => {
    font = newFont;
    showSelect = false;
  };

  const TRANSITION_DURATION = 200; // milliseconds
</script>

<div
  style="font-family:{selectedFont};"
  class="container mx-auto flex items-center flex-col w-full
  max-w-[46.063rem] px-4 lg:px-0 h-screen"
>
  <div class="w-full pt-4 lg:pt-12">
    <div class="relative">
      <div class="mb-12 flex items-center justify-between">
        <BookLogo />
        <div class="flex items-center gap-x-4">
          <div class="flex items-center gap-x-2">
            <p class="text-sm lg:text-lg font-bold capitalize dark:text-white">
              {selectedFont}
            </p>
            <IconArrow onselectFont={() => (showSelect = !showSelect)} />
          </div>
          <div class="bg-[#e9e9e9] w-[0.063rem] h-[2rem]"></div>
          <div class="flex items-center space-x-2 lg:space-x-3">
            <ThemeToggle toggleTheme={changeTheme} id="uno" />
            <ThemeIcon />
          </div>
        </div>
      </div>

      <Dropdown
        {showSelect}
        switchFont={handleFontSwitch}
        {font}
      />
    </div>

    <Input searchWord={searchForWords} />

    {#if searching}
      <div transition:fade={{ duration: TRANSITION_DURATION }}>
        <Skeleton />
      </div>
    {/if}

    {#if meaning.length > 0 && !searching}
      <div transition:fade={{ duration: TRANSITION_DURATION }}>
        <Meanings {meaning} />
      </div>
    {:else if error}
      <div
        transition:slide={{ duration: TRANSITION_DURATION }}
        class="mt-12 text-center flex items-center justify-center flex-col gap-y-4"
      >
        <div class="mb-5">
          <IconEmogi />
        </div>
        <p class="text-[#2D2D2D] dark:text-white font-bold text-[1.25rem]">
          {error.title}
        </p>
        <div>
          <p class="text-[#757575] text-lg font-light">
            {error.message}
          </p>
          <p class="text-[#757575] text-lg font-light">
            {error.resolution}
          </p>
        </div>
      </div>
    {/if}
  </div>
</div>

<style>
  :global(.font-family-selector) {
    background: transparent !important;
  }

  /* Add smooth transitions for theme changes */
  :global(*) {
    transition: background-color 200ms ease-in-out, color 200ms ease-in-out;
  }
</style>
