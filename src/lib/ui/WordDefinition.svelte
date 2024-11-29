<script lang="ts">
  import IconPlay from "../icons/IconPlay.svelte";
  import IconPause from "../icons/IconPause.svelte";
  import PlayButton from "./PlayButton.svelte";
  import IconBrokenAudio from "../icons/IconBrokenAudio.svelte";
  import {
    type Definition,
    type DictionaryResponse,
    type Phonetic,
  } from "../../utils/types";

  let { meaning }: { meaning: DictionaryResponse[] } = $props();
  let audioElement: HTMLAudioElement | null = $state(null);
  let audioState: "play" | "pause" = $state("pause");

  const play = () => {
    if (audioState == "pause") {
      audioState = "play";
      audioElement?.play();
    }
  };

  const stop = () => {
    if (audioState == "play") {
      audioState = "pause";
      audioElement?.pause();
    }
  };

 const getAudioTracks = (data: Array<Phonetic>) => {
      const audios = data
      .map((item) => item?.audio)
      .filter(audio => audio !== "");
      
      return audios.length > 0 
      ? audios[Math.floor(Math.random() * audios.length)]
      : "";
  };

  const mp3Audio = $derived(getAudioTracks(meaning[0]?.phonetics || []));
</script>

{#snippet audioPlayer(phonetics: Phonetic[])}
  <PlayButton handleClick={play}>
    {#if audioState == "pause"}
      <IconPlay />
    {:else}
      <IconPause />
    {/if}
  </PlayButton>

  <audio
    onended={() => stop()}
    bind:this={audioElement}
    src={getAudioTracks(phonetics)}
    typeof="audio/mp3"
    hidden
    autoplay={false}
    controls
  ></audio>
{/snippet}

{#snippet emptyState(phonetic: string)}
  {#if phonetic}
    <div>
      <p class="text-lg lg:text-2xl leading-[1.8125rem] text-[#A445ED]">
        {phonetic}
      </p>
    </div>
  {:else}
    <div>
      <p class="text-lg lg:text-2xl leading-[1.8125rem] text-[#A445ED]">
        No phonetic word
      </p>
    </div>
  {/if}
{/snippet}

{#snippet wordDefinition(definitions: Definition[])}
  <ul class="mt-10 px-4 lg:px-10">
    {#each definitions as { definition, example }}
      <li class="mb-6 text-[#2D2D2D] dark:text-white">
        {definition}

        {#if example}
          <span class="dark:text-white">{example}</span>
        {/if}
      </li>
    {/each}
  </ul>
{/snippet}

{#snippet Synonyms(synonyms: string[])}
  <div class="flex flex-wrap gap-2">
    {#each synonyms.slice(0, 10) as synonym}
      <span
        class="text-[#A445ED] text-base lg:text-lg font-bold hover:underline cursor-pointer"
      >
        {synonym}
      </span>
    {/each}
  </div>
{/snippet}


{#each meaning as { word, sourceUrls, phonetics, phonetic, meanings }}
  <div class="mt-12">
    <div class="flex items-center justify-between w-full">
      <div>
        <h1
          class="text-2xl lg:text-4xl font-bold leading-[5.125rem] capitalize dark:text-white"
        >
          {word}
        </h1>
        {@render emptyState(phonetic)}
      </div>


      {#if mp3Audio}
        {@render audioPlayer(phonetics)}
      {:else}
       
        <PlayButton handleClick={play}>
          <IconBrokenAudio />
        </PlayButton>
    
      {/if}
    </div>
  </div>

  {#each meanings as { definitions, partOfSpeech, synonyms }}
    <div class="mt-12">
      <div class="flex items-center space-x-4 lg:space-x-0 justify-between">
        <p
          class="text-[#2D2D2D] dark:text-white text-lg lg:text-2xl font-bold capitalize"
        >
          {partOfSpeech}
        </p>
        <div
          class="w-full lg:max-w-[41rem] bg-[#E9E9E9] dark:bg-[#3a3a3a] h-px"
        ></div>
      </div>

      <div class="mt-14">
        <p class="text-xl text-[#757575] leading-[0.25rem]">Meaning</p>

        {@render wordDefinition(definitions)}
      </div>
    </div>

    <div class="mt-8">
      <div class="flex items-center space-x-4 flex-wrap">
        <p class="text-base lg:text-lg text-[#757575]">Synonyms</p>
        {@render Synonyms(synonyms)}
      </div>
    </div>
  {/each}
  <div
    class="flex flex-col lg:flex-row lg:items-center lg:space-x-6 my-10 flex-wrap w-full border-t border-[#E9E9E9] dark:border-[#3a3a3a] pt-7"
  >
    <span class="text-[#757575] text-sm lg:text-base">Source</span>
    <p
      class="text-[#2D2D2D] text-sm lg:text-base underline cursor-pointer flex flex-col flex-wrap lg:flex-row w-full"
    >
      <a
        href={sourceUrls[0]}
        class="dark:text-[#ffffff] inline-block"
        target="_blank">{sourceUrls[0]}</a
      >
    </p>
  </div>
{/each}

<style>
  ul li::before {
    content: "•";
    color: #a445ed;
    font-size: 15px;
    margin-right: 15px;
  }
</style>
