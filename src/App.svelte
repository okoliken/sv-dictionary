<script lang="ts">
  import { onMount } from "svelte";
  import SearchInput from "./components/form-elements/SearchInput.svelte";
  import Emogi from "./assets/😕.svg";
  import ThemeIcon from "./components/form-elements/icons/ThemeIcon.svelte";
  import BookLogo from "./components/BookLogo.svelte";
  import Toggle from "./components/form-elements/Toggle.svelte";
  import Arrow from "./components/Arrow.svelte";
  import Dropdown from "./components/Dropdown.svelte";
  import SkFrame from "./components/element-frame/SkFrame.svelte";
  import Meanings from "./components/Meanings.svelte";

  let checked = false;
  let showSelect = false;
  let font = "Lato";
  let searching = false;
  let meaning: any = [];
  let darkMode: boolean | null = false;

  const searchForWords = async (word: any) => {
    searching = true;
    const res = await fetch(
      `https://api.dictionaryapi.dev/api/v2/entries/en/${word.detail}`
    );
    const data = await res.json();

    if (data?.length) meaning = Array(data[0]);
    else meaning = data;

    searching = false;
  };

  const changeTheme = (isToggled: boolean) => {
    localStorage.setItem("isToggled", JSON.stringify(isToggled));
    darkMode = isToggled
  };

 

  onMount(() => {
    darkMode = JSON.parse(localStorage.getItem("isToggled"));

    if (darkMode) {
      document.documentElement.classList.add("dark");
    } else localStorage.setItem("isToggled", JSON.stringify(false));
  });

</script>

<div
  style="font-family:{font};"
  class="container mx-auto h-full flex items-center justify-center flex-col w-full
  max-w-[737px] transition-all duration-150 ease-linear px-6 lg:px-0 dark:bg-[#050505]"
>
  <div class="w-full pt-4 lg:pt-12">
    <div class="relative">
      <div class="mb-12 flex items-center justify-between">
        <BookLogo />
        <div class="flex items-center space-x-2">
          <p
            class="lg:mr-6 text-[14px] lg:text-[18px] font-bold capitalize dark:text-white"
          >
            {font.replace("-", " ") || font}
          </p>
          <Arrow on:selectFont={() => (showSelect = !showSelect)} />
          <div class="bg-[#e9e9e9] w-[1px] h-[32px] transform lg:-translate-x-3" />
          <div class="flex items-center space-x-2 lg:space-x-3">
            <Toggle on:changeTheme={(e) => changeTheme(e.detail)} id="uno" />
            <ThemeIcon />
          </div>
        </div>
      </div>
      {#if showSelect}
        <Dropdown
          on:switchFont={(e) => {
            font = e.detail;
            showSelect = false;
          }}
          {font}
        />
      {/if}
    </div>

    <SearchInput on:searchWord={searchForWords} />

    {#if searching}
      <SkFrame />
    {/if}

    {#if meaning.length > 0 && !searching}
      <div>
        <Meanings {meaning} />
      </div>
    {:else if meaning?.message}
      <div class="mt-12 text-center flex items-center justify-center flex-col">
        <img class="mb-5" src={Emogi} alt="emogi" />
        <p class="text-[#2D2D2D] dark:text-white font-bold text-[20px] mb-12">
          {meaning?.title ?? ""}
        </p>
        <p class="text-[#757575] text-[18px] font-light">
          {meaning?.message ?? ""}
        </p>
        <p class="text-[#757575] text-[18px] font-light">
          {meaning?.resolution ?? ""}
        </p>
      </div>
    {/if}
  </div>
</div>

<style>
  :global(.font-family-selector) {
    background: transparent !important;
  }
</style>
