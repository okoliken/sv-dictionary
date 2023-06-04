<script lang="ts">
  import Play from "../components/form-elements/icons/Play.svelte";
  import Pause from "../components/form-elements/icons/Pause.svelte";
  import PlayButton from "../components/form-elements/PlayButton.svelte";
  export let meaning = [];

  let audioElement;
  let audioState = "pause";

  const play = () => {
    if (audioState == "pause") {
      audioState = "play";
      audioElement.play();
    }
  };

  const stop = () => {
    if (audioState == "play") {
      audioState = "pause";
      audioElement.pause();
    }
  };

  let mp3Audio = "";

  const getAudioTracks = (data: any) => {
    const audios = data.map((item) => {
      if (item?.audio !== "") return item?.audio;
    });

    if (audios.length > 0) {
      return getRandomAudioFromList(audios);
    } else return;
  };

  const getRandomAudioFromList = (list: string[]) => {
    const selectedAudio = Math.floor(Math.random() * list.length);
    return (mp3Audio = list[selectedAudio]);
  };
</script>


{#each meaning as { word, sourceUrls, phonetics, phonetic, meanings }}
  <div class="mt-12">
    <div class="flex items-center justify-between w-full">
      <div>
        <h1
          class="text-[32px] lg:text-[64px] font-bold leading-[82px] capitalize dark:text-white"
        >
          {word}
        </h1>

        {#if phonetic}
          <div>
            <p class="text-[18px] lg:text-[24px] leading-[29px] text-[#A445ED]">
              {phonetic}
            </p>
          </div>
        {:else}
          <div>
            <p class="text-[18px] lg:text-[24px] leading-[29px] text-[#A445ED]">
              No phonetic word
            </p>
          </div>
        {/if}
      </div>

      {#if mp3Audio !== undefined}
        <PlayButton on:click={play}>
          {#if audioState == "pause"}
            <Play />
          {:else}
            <Pause />
          {/if}
        </PlayButton>

        <audio
          on:ended={() => stop()}
          bind:this={audioElement}
          src={getAudioTracks(phonetics)}
          typeof="audio/mp3"
          hidden
          autoplay={false}
          controls
        />
        {:else}
        <p class="dark:text-white text-[14px] lg:text-[18x]">No audio</p>
      {/if}
    </div>
  </div>

  {#each meanings as { definitions, partOfSpeech, synonyms }}
    <div class="mt-12">
      <div class="flex items-center space-x-4 lg:space-x-0 justify-between">
        <p
          class="text-[#2D2D2D] dark:text-white text-[18px] lg:text-[24px] font-bold capitalize"
        >
          {partOfSpeech}
        </p>
        <div
          class="w-full lg:max-w-[656px] bg-[#E9E9E9] dark:bg-[#3a3a3a] h-[1px]"
        />
      </div>

      <div class="mt-14">
        <p class="text-[20px] text-[#757575] leading-[4px]">Meaning</p>

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
      </div>
    </div>

    <div class="mt-8">
      <div class="flex items-center space-x-4 flex-wrap">
        <p class="text-[16px] lg:text-[20px] text-[#757575]">Synonyms</p>
        {#each synonyms as synonyms}
          <p
            class="text-[#A445ED] text-[16px] text-justify lg:text-[20px] font-bold hover:underline cursor-pointer"
          >
            {synonyms}
          </p>
        {/each}
      </div>
    </div>
  {/each}
  <div
    class="flex flex-col lg:flex-row lg:items-center lg:space-x-6 my-10 flex-wrap w-full border-t border-[#E9E9E9] dark:border-[#3a3a3a] pt-7"
  >
    <span class="text-[#757575] text-[14px]">Source</span>
    <p
      class="text-[#2D2D2D] text-[14px] underline cursor-pointer flex flex-col flex-wrap lg:flex-row w-full"
    >
      <a href={sourceUrls[0]} class="dark:text-[#ffffff] inline-block" target="_blank"
        >{sourceUrls[0]}</a
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
