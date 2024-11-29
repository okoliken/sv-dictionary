<script lang="ts">
    import { scale } from "svelte/transition";
    import { quintOut } from "svelte/easing";
    import SearchIcon from "../icons/SearchIcon.svelte";
  
    let { searchWord }: { searchWord: (word: string) => Promise<void> } = $props();
    let value: string = $state('');
    let touched: boolean = $state(false);
    
    const isInvalid = $derived(touched && !value.trim());

    const handleSubmit = (e: SubmitEvent) => {
        e.preventDefault();
        touched = true;
        
        if (value.trim()) {
            searchWord(value);
        }
    };
</script>

<form
    onsubmit={handleSubmit}
    class="relative sm:max-w-[736px] w-full flex items-center"
    class:error={isInvalid}
>
    <input
        bind:value
        type="text"
        placeholder="Search for a word"
        class="w-full max-w-full min-h-[48px] sm:max-w-[736px] sm:min-h-[64px] 
               px-4 lg:px-6 rounded-2xl font-bold text-base lg:text-xl
               bg-gray-100 dark:bg-neutral-800 dark:text-white 
               outline-none appearance-none
               placeholder:font-normal placeholder:text-base
               focus:border focus:border-primary
               {isInvalid ? 'border border-red-500' : 'border-transparent'}"
        aria-invalid={isInvalid}
        aria-errormessage="error-message"
    />
    <button 
        type="submit"
        aria-label="Search"
        class="absolute top-3.5 lg:top-[1.35rem] right-6"
    >
        <SearchIcon />
    </button>
</form>

{#if isInvalid}
    <p
        transition:scale={{ delay: 50, duration: 300, easing: quintOut }}
        id="error-message"
        role="alert"
        class="text-red-500 mt-1 text-base sm:text-sm"
    >
        Whoops, can't be empty…
    </p>
{/if}

<style lang="postcss">
    :global(.error svg) {
        @apply text-red-500;
    }
</style>