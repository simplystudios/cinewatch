<script lang="ts">
  // @ts-nocheck

  import { Button } from "$lib/components/ui/button";
  import * as Card from "$lib/components/ui/card";
  import { Star } from 'lucide-svelte';
  import Layout from "./+layout.svelte";
  import { Twitter } from 'lucide-svelte';
  import embla from "svelte-embla";
  import { setMode, resetMode, mode } from "mode-watcher";
  import { CalendarDays } from 'lucide-svelte';
  import { navigating } from '$app/stores'
  import { Pulse } from 'svelte-loading-spinners';
  import { Coffee } from 'lucide-svelte';
  import * as Alert from "$lib/components/ui/alert";
  import Separator from "$lib/components/ui/separator/separator.svelte";
  import { Input } from "$lib/components/ui/input";
  import * as DropdownMenu from "$lib/components/ui/dropdown-menu";
  import { Github } from 'lucide-svelte';
  import { Badge } from "$lib/components/ui/badge";
  import * as Accordion from "$lib/components/ui/accordion";
  import * as Dialog from "$lib/components/ui/dialog";
  import { Calendar } from "bits-ui";
  import { onMount } from "svelte";

  let searchterm = "";
  let searchd=[];
  let searchcss = "flex items-center justify-center p-10 pt-10 pb-10 mb-10";
  let cssp = "";
  let allseasons = { seasons: [] }; // Initialize with empty seasons array
  let logoshow;
  let popdata=[];
  let dialogOpen = false;
  let hid = "flex justify-center pt-20";
  let covermage="";
  let genreids=[];
  let hid2 = "flex justify-center";
  let netflixshows=[];
  let epnum = 0;
  let showid
  let dialogPlayer = false;
  let serverd = [];
  let infoj = [];
  let eplist = [];
  let dumb={}
  let allseasonsdata = {};
  let dumbvideourl
  let playerurl;
  let popshow =[];
  let dumbvideos=[];

  const ggs = async (data, type) => {
    window.open(`/info?id=${data}?type=${type}`, "_self");
  };

  const func = async () => {
    const response = await fetch(`https://consumetmuyi.vercel.app/meta/tmdb/${searchterm}`);
    searchd = await response.json();
    searchd = searchd.results;
    searchcss = "flex items-center justify-center pb-10 pt-4 mb-auto";
    console.log(searchd);
  };

  onMount(async () => {
    try {
      const [popResponse, tvResponse, netflixResponse] = await Promise.all([
        fetch("https://api.themoviedb.org/3/movie/popular?api_key=07d7cff6553ffe45f88ee4c89a93a12c"),
        fetch("https://api.themoviedb.org/3/tv/top_rated?api_key=07d7cff6553ffe45f88ee4c89a93a12c&limit=4"),
        fetch("https://api.themoviedb.org/3/discover/tv?api_key=07d7cff6553ffe45f88ee4c89a93a12c&with_networks=213")
      ]);

      // Popular movies
      try {
        const popJson = await popResponse.json();
        popdata = popJson.results.slice(0, 4);
        const randomIndex = Math.floor(Math.random() * popJson.results.length);
        dumb = popJson.results[randomIndex];
        covermage = `https://image.tmdb.org/t/p/w1280/${dumb.backdrop_path}`;

        const dumbVideoResponse = await fetch(`https://api.themoviedb.org/3/movie/${dumb.id}/videos?api_key=07d7cff6553ffe45f88ee4c89a93a12c`);
        const dumbVideoJson = await dumbVideoResponse.json();
        if (dumbVideoJson.results) {
          dumbvideourl = `https://www.youtube.com/embed/${dumbVideoJson.results[5].key}?autoplay=1&controls=0&loop=0&mute=1&modestbranding=1&autohide=1&showinfo=0&rel=0`;
        }
        console.log(dumbvideourl);
      } catch (error) {
        console.error("Error fetching popular movies or videos:", error);
      }

      // Top-rated TV shows
      try {
        const tvJson = await tvResponse.json();
        popshow = tvJson.results.slice(0, 4);
      } catch (error) {
        console.error("Error fetching top-rated TV shows:", error);
      }

      // Netflix Originals
      try {
        const netflixJson = await netflixResponse.json();
        netflixshows = netflixJson.results;
      } catch (error) {
        console.error("Error fetching Netflix Originals:", error);
      }

    } catch (error) {
      console.error("Error fetching data:", error);
    }
  });
</script>

{#if $navigating}
	<Pulse size="60" color="#FF3E00" unit="px" duration="1s" />
{/if}

<div class="max-h-max">
   <div class="relative h-[80vw] md:h-[56.25vw] object-cover content-center overflow-hidden">
        {#if dumbvideos.length>0}
    <iframe class="pointer-events-none overflow-hidden w-full h-[56.25vw] brightness-[50%] transform scale-125" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen src={dumbvideourl} alt="">
    </iframe>
    {:else}
      <img src={`https://image.tmdb.org/t/p/w1280/${dumb.backdrop_path}`} class="pointer-events-none overflow-hidden w-full h-[80vw] md:h-[56.25vw] brightness-[50%] object-cover transform scale-125" alt="">
    {/if}
</div>
<div style="background: url(data:image/svg+xml;base64,alotofcodehere);
    background: -moz-linear-gradient(top,   rgba(255,255,255,0) 0%, rgba(18,18,18,255) 70%);
    background: -webkit-gradient(linear, left top, left bottom, color-stop(0%,rgba(255,255,255,0)), color-stop(70%,#000000));
    background: -webkit-linear-gradient(top,  rgba(255,255,255,0) 0%, rgba(18,18,18,255) 70%);
    background: -o-linear-gradient(top,   rgba(255,255,255,0) 0%, rgba(18,18,18,255) 70%);
    background: -ms-linear-gradient(top,  rgba(255,255,255,0) 0%, rgba(18,18,18,255) 70%);
    background: linear-gradient(to bottom,  rgba(255,255,255,0) 0%, rgba(18,18,18,255) 70%);"  class="h-[65vw] lg:h-[41vw] md:h-[45vw]  sm:h-[45vw] w-full absolute top-[10%] md:top-[30%] ml-0 md:ml-0">
    <div class="text-center sm:ml-6 sm:text-left">
      <div>
        <p class="text-white text-3xl md:text-5xl h-full w-[100%] lg:text-5xl font-bold drop-shadow-xl">{dumb.title}</p>
        <div class="text-center md:text-left">
          <p class="text-[8px] sm:text-left w-[50%] mt-4 sm:text-[14px]">{dumb.overview}</p>
        </div>
        <p class="text=[10px] text-green-600 mt-2 font-bold ">90% Match</p>
        <Button class="mt-5 w-36 tex">More Info</Button>
        <Button variant="secondary" class="mt-5 w-36">Watch Now</Button>
      </div>
    </div>
</div>


  {#if searchd.length > 0}
    <h2 class="text-2xl font-bold ml-2 mb-5">Search Results</h2>
    <div class="group grid grid-cols-2 gap-4 md:flex overflow-x-scroll">
      {#each searchd as d}
        <button on:click={() => ggs(d.id, d.type)}>
          <div class="w-[190px] h-[340px] ml-2 mr-2">
            <img class="object-cover h-auto w-auto" src={d.image} alt="">
          </div>
        </button>
      {/each}
    </div>
  {/if}



  <div class="px-4 md:px-12 mt-4 space-y-8">
    <h2 class="text-2xl font-bold ml-1 mt-10">Top Tv Shows</h2>
    {#if popshow.length>0}
    <div class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 gap-2">
      {#each popshow as d}
        <div on:click={() => ggs(d.id, "Tv Series")} class="group">
          <div class="transform transition duration-500 hover:scale-110">
            <img class="object-cover w-full h-[24vw] md:h-[12vw] shadow-xl cursor-pointer transition duration group-hover:opacity-90 sm:group-hover:opacity-0 delay-100" src={`https://image.tmdb.org/t/p/w500/${d.backdrop_path}`} alt="">
            <div class="opacity-0 absolute top-0 transition duration-200 z-10 invisible sm:visible delay-300 w-full scale-0 group-hover:scale-110 group-hover:-translate-y-[6vw] group-hover:-translate-x-[1.4vw] group-hover:opacity-100">
              <img class="object-cover w-full h-[24vw] md:h-[12vw] shadow-xl cursor-pointer transition rounded-t-md duration" src={`https://image.tmdb.org/t/p/w500/${d.backdrop_path}`} alt="">
              <div class="z-10 bg-zinc-800 p-2 lg:p-4 absolute w-full transition shadow-md rounded-b-md">
                <div class="flex flex-row items-center gap-3"></div>
                <h1 class="font-bold mb-1">{d.name}</h1>
                <p class="text-sm line-clamp-2">{d.overview}</p>
              </div>
            </div>
          </div>
        </div>
      {/each}
    </div>
     {:else}
  <Pulse size="60" color="#FF3E00" unit="px" duration="1s" />
  {/if}
  </div>


  <div class="px-4 md:px-12 mt-20 space-y-8 z-0 mb-10">
    <h2 class="text-2xl font-bold ml-1 mb-5 z-0">Popular On Cinewatch</h2>
    {#if popdata.length>0}
    <div class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 gap-2">
      {#each popdata as d}
        <div on:click={() => ggs(d.id, "Movie")} class="group">
          <div class="transform transition duration-500 hover:scale-110">
            <img class="object-cover w-full h-[294px] md:h-[320px] shadow-xl cursor-pointer transition duration group-hover:opacity-90 sm:group-hover:opacity-0 delay-100" src={`https://image.tmdb.org/t/p/w500/${d.poster_path}`} alt="">
            <div class="opacity-0 absolute top-0 transition duration-200 z-10 invisible sm:visible delay-300 w-full scale-0 group-hover:scale-110 group-hover:-translate-y-[6vw] group-hover:-translate-x-[1.4vw] group-hover:opacity-100">
              <img class="object-cover w-full h-[294px] md:h-[320px] shadow-xl cursor-pointer transition rounded-t-md duration" src={`https://image.tmdb.org/t/p/w500/${d.poster_path}`} alt="">
              <div class="z-10 bg-zinc-800 p-2 lg:p-4 absolute w-full transition shadow-md rounded-b-md">
                <div class="flex flex-row items-center gap-3"></div>
                <h1 class="font-bold mb-1 line-clamp-1">{d.title}</h1>
                <p class="text-sm line-clamp-2">{d.overview}</p>
              </div>
            </div>
          </div>
        </div>
      {/each}
    </div>
      {:else}
  <Pulse size="60" color="#FF3E00" unit="px" duration="1s" />

{/if}
  </div>

  <div class="px-4 md:px-12 mt-30 space-y-8">
    <h2 class="text-2xl font-bold ml-1">Netflix Originals</h2>
    {#if netflixshows.length>0}
    <div class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 gap-2">
      {#each netflixshows as d}
        <div on:click={() => ggs(d.id, "Movie")} class="group">
          <div class="transform transition duration-500 hover:scale-110">
            <img class="object-cover w-full h-[294px] md:h-[320px] shadow-xl cursor-pointer transition duration group-hover:opacity-90 sm:group-hover:opacity-0 delay-100" src={`https://image.tmdb.org/t/p/w500/${d.poster_path}`} alt="">
            <div class="opacity-0 absolute top-0 transition duration-200 z-10 invisible sm:visible delay-300 w-full scale-0 group-hover:scale-110 group-hover:-translate-y-[6vw] group-hover:-translate-x-[1.4vw] group-hover:opacity-100">
              <img class="object-cover w-full h-[294px] md:h-[320px] shadow-xl cursor-pointer transition rounded-t-md duration" src={`https://image.tmdb.org/t/p/w500/${d.poster_path}`} alt="">
              <div class="z-10 bg-zinc-800 p-2 lg:p-4 absolute w-full transition shadow-md rounded-b-md">
                <div class="flex flex-row items-center gap-3"></div>
                <h1 class="font-bold mb-1 line-clamp-1">{d.name}</h1>
                <p class="text-sm line-clamp-2">{d.overview}</p>
              </div>
            </div>
          </div>
        </div>
      {/each}
    </div>
      {:else}
    <Pulse size="60" color="#FF3E00" unit="px" duration="1s" />
{/if}
  </div>

  <footer class="h-max mt-40 bottom-auto top-auto mb-3">
    <Separator orientation="horizontal" />
    <div class="flex pr-8 justify-between">
      <div class="ml-2 mr-20">
        <p class="font-bold">Contact Us</p>
        <br>
        <div class="flex space-x-2">
          <a href="https://twitter.com/anshwadhwa8">
            <Twitter />
          </a>
          <a href="https://buymeacoffee.com/anshwadhwa8">
            <Coffee />
          </a>
          <a href="https://github.com/simplystudios">
            <Github />
          </a>
        </div>
      </div>
      <div class="text-right bottom-0 left-0 float-right ml-10">
        <h1 class="font-bold mb-1">About</h1>
        <p class="text-right bottom-0 left-0 float-right ml-10">Made By Ansh Wadhwa</p>
      </div>
    </div>
  </footer>
</div>

<style>
  ::-webkit-scrollbar {
    width: 6px;
    height: 4px;
  }

  ::-webkit-scrollbar-track {
    background: transparent;
  }

  ::-webkit-scrollbar-thumb {
    background: #3a3a3a;
  }

  ::-webkit-scrollbar-thumb:hover {
    background: #555;
  }

  .content-wrapper:hover .overlay-content {
    display: block;
  }

  .overlay-content {
    display: none;
  }

  .embla {
    overflow: hidden;
  }

  .embla__container {
    display: flex;
  }

  .embla__slide {
    flex: 0 0 100%;
    min-width: 0;
  }
</style>
