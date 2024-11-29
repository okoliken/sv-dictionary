<div class="pt-12 cursor-wait">
  <div class="flex items-center justify-between">
    <div class="space-y-3">
      <div class="skeleton w-32 lg:w-52 h-8 lg:h-12 rounded-md"></div>

      <div class="skeleton w-20 h-6 rounded-md"></div>
    </div>
    <div class="skeleton w-12 h-12 lg:w-[4.5rem] lg:h-[4.5rem] rounded-full"></div>
  </div>

  {#each Array(2) as _}
    <div class="mt-12 space-y-8">
      <div class="flex items-center gap-4">
        <div class="skeleton w-24 h-6 rounded-md"></div>
        <div class="skeleton flex-1 lg:max-w-[41rem] h-px"></div>
      </div>

      <div class="skeleton w-20 h-5 rounded-md"></div>

      <ul class="space-y-4 mt-6">
        {#each Array(5) as _}
          <div class="flex items-center gap-4">
            <div class="skeleton w-2 h-2 lg:w-3 lg:h-3 rounded-full shrink-0"></div>
            <div class="skeleton w-full h-4 rounded-md"></div>
          </div>
        {/each}
      </ul>
    </div>
  {/each}
</div>

<style lang="postcss">  
  .skeleton {
    @apply relative overflow-hidden bg-gray-200/70;
  }

  :global(.dark) .skeleton {
    @apply bg-neutral-800/50;
  }

  .skeleton::after {
    @apply absolute inset-0;
    content: "";
    background-image: linear-gradient(
      90deg,
      transparent 0%,
      theme(colors.gray.500/40%) 50%,
      transparent 100%
    );
    background-size: 200% 100%;
    animation: shimmer 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
  }

  :global(.dark .skeleton)::after {
    background-image: linear-gradient(
      90deg,
      transparent 0%,
      theme(colors.neutral.700/30%) 50%,
      transparent 100%
    );
  }

  @keyframes shimmer {
    0% {
      background-position: -200% 0;
    }
    100% {
      background-position: 200% 0;
    }
  }
</style>
