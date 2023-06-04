<script lang="ts">

  const btnStyle =
    "w-[48px] lg:w-[75px] play-button h-[48px] lg:h-[75px] bg-[#e8d1fb] dark:bg-[#2d163f] flex items-center justify-center rounded-full duration-100 ease-in-out transition";
</script>

<button on:click class={btnStyle} >
  <slot>click me</slot>
</button>

<style>
  :global(.play-button:hover svg){
    color: #ffffff;
  }
  .play-button:hover{
    background: #A445ED;
  }
  
</style>
