<div class="pt-12 cursor-wait">
  <div class="flex items-center justify-between">
    <div>
      <div class="bg-gray-200 dark:bg-[#1f1f1f] animate-pulse w-32 lg:w-52 h-5 lg:h-10 rounded" />

      <div class="bg-gray-200 dark:bg-[#1f1f1f] animate-pulse w-20 h-5 rounded mt-3" />
    </div>
    <div
      class="bg-gray-200 dark:bg-[#1f1f1f] animate-pulse w-[48px] h-[48px] lg:w-[75px] lg:h-[75px] rounded-full"
    />
  </div>

  {#each Array(2) as definitions}
    <div class="mt-12">
      <div class="flex items-center justify-between">
        <div class="w-[38px] h-[13px] bg-gray-200 dark:bg-[#1f1f1f] animate-pulse" />
        <div
          class="animate-pulse w-full lg:max-w-[656px] bg-[#E9E9E9] dark:bg-[#1f1f1f] h-[1px]"
        />
      </div>
      <div class="w-[72px] h-[13px] bg-gray-200 dark:bg-[#1f1f1f] animate-pulse mt-12" />

      <ul class="mt-12">
        {#each Array(5) as list}
          <div class="flex items-center space-x-4 mb-5">
            <div
              class="bg-gray-200 dark:bg-[#1f1f1f] animate-pulse w-[8px] h-[8px] lg:w-[16px] lg:h-[16px] rounded-full"
            />
            <li class="w-full h-[10px] bg-gray-200 dark:bg-[#1f1f1f] animate-pulse" />
          </div>
        {/each}
      </ul>
    </div>
  {/each}
</div>
