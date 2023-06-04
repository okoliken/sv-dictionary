<script lang="ts">
  import { onMount, createEventDispatcher } from "svelte";
  import { fade } from "svelte/transition";

  const dispatch = createEventDispatcher();

  {
    (""); // keep something to prevent Vite empty block sourcemap bug
  }

  let darkMode = null;
  let darkModeReady = false;
  export let disabled = false;

  const changeTheme = (val: boolean) => {
    document.documentElement.classList.toggle("dark");
    dispatch("changeTheme", val);
  };

  onMount(() => {

    darkMode = JSON.parse(localStorage.getItem("isToggled"));

    if (darkMode) {
      document.documentElement.classList.add("dark");
    } else localStorage.setItem("isToggled", JSON.stringify(false));

    darkMode = document.documentElement.classList.contains("dark");
    document.body.classList.add("transition", "ease-in-out", "duration-500");
    darkModeReady = true;
  });

  export let id = "";
</script>

{#if darkModeReady}
  <label for={id}>
    <div class="switch">
      <input
        {id}
        name={id}
        type="checkbox"
        class="sr-only"
        {disabled}
        on:input={(e) => changeTheme(e.currentTarget.checked)}
        bind:checked={darkMode}
      />
      <div class="track" />
      <div class="thumb" />
    </div>
  </label>
{/if}

<style lang="postcss">
  .switch {
    @apply relative inline-block align-middle cursor-pointer select-none bg-transparent;
  }

  .track {
    @apply w-10 lg:w-12 h-5 lg:h-6 bg-[#757575] rounded-full shadow-inner;
  }

  .thumb {
    @apply transition-all duration-300 ease-in-out absolute top-0 left-0 w-5 h-5 lg:w-6 lg:h-6 bg-white border-2 border-[#757575] rounded-full;
  }

  input[type="checkbox"]:checked ~ .thumb {
    @apply transform translate-x-full border-[#a445ed];
  }

  input[type="checkbox"]:checked ~ .track {
    @apply transform transition-colors bg-[#a445ed];
  }

  input[type="checkbox"]:disabled ~ .track {
    @apply bg-[#757575];
  }

  input[type="checkbox"]:disabled ~ .thumb {
    @apply bg-gray-100 border-[#757575];
  }

  input[type="checkbox"]:focus + .track,
  input[type="checkbox"]:active + .track {
    @apply shadow;
  }
</style>
