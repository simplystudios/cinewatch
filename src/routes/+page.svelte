<script lang="ts">
  // @ts-nocheck
  import { baseurl } from '$lib/ss';
  import { Button } from "$lib/components/ui/button";
  import * as Card from "$lib/components/ui/card";
  import { MoveDown } from 'lucide-svelte';
  import Layout from "./+layout.svelte";
  import { Twitter } from 'lucide-svelte';
  import embla from "svelte-embla";
  import { setMode, resetMode, mode } from "mode-watcher";
  import { CalendarDays } from 'lucide-svelte';
  import { navigating } from '$app/stores'
  import { Pulse } from 'svelte-loading-spinners';
  import { Coffee } from 'lucide-svelte';
  import { Plus } from 'lucide-svelte';
  import { Play } from 'lucide-svelte';
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
  let dumbimgs = [];
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
  const play = async (data) => {
    window.open(`/watch?id=${data}`, "_self");
  };

  const func = async () => {
    const response = await fetch(`${baseurl}/meta/tmdb/${searchterm}`);
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
        popdata = popJson.results.slice(0, 5);
        const randomIndex = Math.floor(Math.random() * popJson.results.length);
        dumb = popJson.results[randomIndex];
        covermage = `https://image.tmdb.org/t/p/w1280/${dumb.backdrop_path}`;

       const dumbVideoResponse = await fetch(`https://api.themoviedb.org/3/movie/${dumb.id}/videos?api_key=07d7cff6553ffe45f88ee4c89a93a12c`);
const dumbVideoJson = await dumbVideoResponse.json();

let dumblogoResponse = await fetch(`https://api.themoviedb.org/3/movie/${dumb.id}/images?api_key=07d7cff6553ffe45f88ee4c89a93a12c`);
let dumblogoJson = await dumblogoResponse.json();

// Filter logos to get only the ones with English language ("en")
let englishLogos = dumblogoJson.logos.filter(logo => logo.iso_639_1 === "en");

// Select the first English logo
        let dumbLogo = englishLogos.length > 0 ? englishLogos[0] : null;
        dumbimgs = dumbLogo.file_path

// Log the result
console.log(dumbLogo);

        dumbvideos = dumbVideoJson.results;
        if (dumbVideoJson.results) {
          dumbvideourl = `https://www.youtube.com/embed/${dumbVideoJson.results[0].key}?autoplay=1&controls=0&playlist=${dumbVideoJson.results[0].key}&loop=1&mute=1&modestbranding=1&autohide=1&showinfo=0&rel=0`;
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
<Layout/>

{#if $navigating}
	<Pulse size="60" color="#FF3E00" unit="px" duration="100s" />
{/if}

<div class="">
   <div class="relative h-[85vw] sm:h-[80vw] md:h-[56.25vw] object-cover content-center overflow-hidden">
        {#if dumbvideourl}
    <iframe class="pointer-events-none overflow-hidden w-full h-full brightness-[50%] transform scale-125" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen src={dumbvideourl} alt="">
    </iframe>
    {:else}
      <img src={`https://image.tmdb.org/t/p/w1280/${dumb.backdrop_path}`} class="pointer-events-none overflow-hidden w-full h-[40vw] md:h-[56.25vw] brightness-[50%] object-cover transform scale-125" alt="">
    {/if}
</div>
<div style="background: url(data:image/svg+xml;base64,alotofcodehere);
    background: -moz-linear-gradient(top,   rgba(255,255,255,0) 0%, rgba(18,18,18,255) 70%);
    background: -webkit-gradient(linear, left top, left bottom, color-stop(0%,rgba(255,255,255,0)), color-stop(70%,#000000));
    background: -webkit-linear-gradient(top,  rgba(255,255,255,0) 0%, rgba(18,18,18,255) 70%);
    background: -o-linear-gradient(top,   rgba(255,255,255,0) 0%, rgba(18,18,18,255) 70%);
    background: -ms-linear-gradient(top,  rgba(255,255,255,0) 0%, rgba(18,18,18,255) 70%);
    background: linear-gradient(to bottom,  rgba(255,255,255,0) 0%, rgba(18,18,18,255) 70%);"  class="z-0 h-[67vw] lg:h-[30vw] md:h-[37vw]  sm:h-[50vw] w-full absolute top-[28%] sm:top-[40%] md:top-[28.8%] lg:top-[54%] xl:top-[65%] ml-0 md:ml-0">
    <div class="text-center sm:ml-6 sm:text-left">
      <div>
        <div class="flex justify-center sm:justify-start">
           <img src={`https://image.tmdb.org/t/p/w500/${dumbimgs}`} class="h-[100px] max-w-[300px] object-center sm:ml-0 sm:mr-0"/>
        </div>
        <div class="text-center md:text-left">
          <p class="w-auto lg:w-[50%] text-sm sm:text-left line-clamp-4 mt-4 ">{dumb.overview}</p>
        </div>
        <Button on:click={()=>ggs(dumb.id, "Movie")} class="mt-5 w-36 tex">More Info</Button>
        <Button on:click={()=>play(dumb.id)} variant="secondary" class="mt-5 w-36">Watch Now</Button>
      </div>
    </div>
</div>

  <div class="px-4 md:px-12 mt-20 sm:mt-30 md:mt-16 lg:mt-16 space-y-8">
    <h2 class="text-2xl z-10 relative font-bold ml-1 mt-10">Top Tv Shows</h2>
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
                <p class="text-sm line-clamp-3">{d.overview}</p>
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
    <div class="grid h-full grid-cols-2 sm:grid-cols-3 md:grid-cols-5 ">
      {#each popdata as d}
        <div on:click={() => ggs(d.id, "Movie")} class="group">
          <div class="transform transition duration-500 hover:scale-110">
            <img class="object-center w-12/12 h-12/12 md:h-12/12 shadow-xl cursor-pointer transition duration group-hover:opacity-90 sm:group-hover:opacity-0 delay-100" src={`https://image.tmdb.org/t/p/w500/${d.poster_path}`} alt="">
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
  {#if netflixshows.length > 0}
  <div class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-5 gap-2">
    {#each netflixshows as d}
      <div on:click={() => ggs(d.id, "TV Series")} class="group relative">  <img class="object-cover w-[210px] h-[320px] md:h-[320px] shadow-xl cursor-pointer transition duration group-hover:opacity-90 sm:group-hover:opacity-0 delay-100" src={`https://image.tmdb.org/t/p/w500/${d.poster_path}`} alt="">
        <div class="opacity-0 absolute top-0 transition duration-200 z-30 invisible sm:visible delay-300 w-full scale-0 group-hover:scale-110 group-hover:-translate-y-[6vw] group-hover:-translate-x-[1.4vw] group-hover:opacity-100">
          <img class="object-cover w-12/12 h-12/12 md:h-12/12  shadow-xl cursor-pointer transition rounded-t-md duration" src={`https://image.tmdb.org/t/p/w500/${d.poster_path}`} alt="">
          <div class="z-10 bg-zinc-800 p-2 lg:p-4 absolute w-full transition shadow-md rounded-b-md">
            <div class="flex flex-row items-center gap-3">
              <button class="bg-white rounded-[100%] w-[30px] h-[30px] text-center">
                <Play color="black" size="16px" class="ml-[7px]"/>
              </button>
              <button class="bg-white rounded-[100%] w-[30px] h-[30px] text-center">
                <Plus color="black" size="16px" class="ml-[7px]"/>
              </button>
            </div>
            <h1 class="font-bold mb-1 line-clamp-1">{d.name}</h1>
            <p class="text-sm line-clamp-2">{d.overview}</p>
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
