<script lang="ts">
    import {onMount} from 'svelte'
  let darkMode: boolean = $state(false);
  let darkModeReady = $state(false);

  let {
    id = "",
    toggleTheme,
  }: { id: string; toggleTheme: (val: boolean) => void } = $props();

  const changeTheme = (val: boolean) => {
    darkMode = val;
    document.documentElement.classList.toggle("dark", val);
    localStorage.setItem("isToggled", JSON.stringify(val));
    toggleTheme(val);
  };

  onMount(() => {
    // Get stored preference, default to false if not set
    const storedPreference = localStorage.getItem("isToggled");
    const isDark = storedPreference ? JSON.parse(storedPreference) : false;
    
    // Set initial state
    changeTheme(isDark);
    darkModeReady = true;
  });
</script>

{#if darkModeReady}
  <label for={id}>
    <div class="switch">
      <input
        {id}
        name={id}
        type="checkbox"
        class="sr-only"
        oninput={(e) => changeTheme(e.currentTarget.checked)}
        checked={darkMode}
      />
      <div class="track"></div>
      <div class="thumb"></div>
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
