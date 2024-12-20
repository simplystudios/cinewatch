<script>
// @ts-nocheck

import { Button } from "$lib/components/ui/button";
  import * as Card from "$lib/components/ui/card";
  import { Skeleton } from "$lib/components/ui/skeleton/index.js"
  import { baseurl } from '$lib/ss';
   import { Pulse } from 'svelte-loading-spinners';
  import Layout from "../+layout.svelte";
  import { onMount } from 'svelte';
  import { Twitter } from 'lucide-svelte';
  import { setMode, resetMode, mode } from "mode-watcher";
  import { CalendarDays, Tv } from 'lucide-svelte';
  import * as DropdownMenu from "$lib/components/ui/dropdown-menu";
  import { Github } from 'lucide-svelte';
  import { Badge } from "$lib/components/ui/badge";
  let searchterm = "";
let searchd = [];
let searchcss = "flex items-center justify-center p-40 pt-10 pb-20 mb-10";
let cssp = "";
let pp;
let dumbimgs
let allseasons = { seasons: [] }; // Initialize with empty seasons array
let logoshow;
let dialogOpen = false;
let hid = "flex justify-center pt-20";
let id;
let hid2 = "flex justify-center";
let epnum = 0;
let showid;
let dialogPlayer = false;
let serverd = [];
let pimg;
let pcover;
let pi = [];
let eplist = [];
let allseasonsdata = {};
let playerurl;
let mid;
let type;
let seasonid=0;
let pageurl = '';
let loading = true;

 onMount(async() =>{
    pageurl=window.location.search;
    pageurl = pageurl.replace(/\?(?=[^?]*$)/, "&");

// Parse the corrected query string
    const params = new URLSearchParams(pageurl);
    mid = params.get("id");
    type = params.get("type");
    console.log(pageurl);
    console.log(type)
    if (type == "Movie"){
      try {
      const api = `https://api.themoviedb.org/3/movie/${mid}?api_key=07d7cff6553ffe45f88ee4c89a93a12c`;
      const response = await fetch(api);
      pi = await response.json();
      console.log(pi);
      pimg = `https://image.tmdb.org/t/p/w1280/${pi.poster_path}`
      pcover = `https://image.tmdb.org/t/p/w1280/${pi.backdrop_path}`
      let dumblogoResponse = await fetch(`https://api.themoviedb.org/3/movie/${mid}/images?api_key=07d7cff6553ffe45f88ee4c89a93a12c`);
    let dumblogoJson = await dumblogoResponse.json();

    // Filter logos to get only the ones with English language ("en")
      let englishLogos = dumblogoJson.logos.filter(logo => logo.iso_639_1 === "en");

// Select the first English logo
        let dumbLogo = englishLogos.length > 0 ? englishLogos[0] : null;
        dumbimgs = dumbLogo.file_path
        dumbimgs = `https://image.tmdb.org/t/p/w500/${dumbimgs}`

// Log the result
    console.log(dumbLogo);
loading = false;
    } catch (error) {
      console.error("Error fetching data:", error);
    } finally {
      loading = false; // Data fetching complete
    }
    }
    else if (type=="Tv Series"){
        
        const api2 = `https://api.themoviedb.org/3/tv/${mid}?api_key=07d7cff6553ffe45f88ee4c89a93a12c`;
        const response2 = await fetch(api2);
        pi = await response2.json()
        pimg = `https://image.tmdb.org/t/p/w1280/${pi.poster_path}`
      pcover = `https://image.tmdb.org/t/p/w1280/${pi.backdrop_path}`
       allseasons = pi.seasons;
       showid = pi.id;
       console.log(allseasons)
       seasonchange(1)
       let dumblogoResponse = await fetch(`https://api.themoviedb.org/3/tv/${mid}/images?api_key=07d7cff6553ffe45f88ee4c89a93a12c`);
    let dumblogoJson = await dumblogoResponse.json();

    // Filter logos to get only the ones with English language ("en")
      let englishLogos = dumblogoJson.logos.filter(logo => logo.iso_639_1 === "en");

// Select the first English logo
        let dumbLogo = englishLogos.length > 0 ? englishLogos[0] : null;
        dumbimgs = dumbLogo.file_path
        dumbimgs = `https://image.tmdb.org/t/p/original/${dumbimgs}`

// Log the result
console.log(dumbLogo);
    }
  })


const seasonchange = async(no) => {
    let seasono = no;
    console.log(seasono);
    const ap = `https://api.themoviedb.org/3/tv/${mid}/season/${seasono}?api_key=07d7cff6553ffe45f88ee4c89a93a12c`
    const resp = await fetch(ap)
    allseasonsdata= await resp.json()
    console.log(allseasonsdata);
    eplist = allseasonsdata.episodes;
    console.log(eplist)
    loading=false;
};

const playep = (id, season, ep) => {
    window.open(`/watch?id=${id}?se=${season}-${ep}`);
};

const playnextep = (id, season, ep, type) => {
    window.open(`/watch?id=${id}?se=${season}-${ep}?type=${type}`);
};

const openplayer = async (id, type) => {
    if (type == "Tv Series") {
        window.open(`/watch?id=${id}?se=1-1?type=${type}`);
    } else {
        window.open(`/watch?id=${id}`);
    }
};

const func = async () => {
    try {
        const response = await fetch(`https://api.themoviedb.org/3/search/multi?api_key=${tmdbApiKey}&query=${searchterm}`);
        const searchData = await response.json();
        searchcss = "flex items-center justify-center pb-10 pt-4 mb-auto";
        cssp = "hidden";
        hid = "hidden";
        hid2 = "hidden";
        searchd = searchData.results;
    } catch (error) {
        console.error("Error fetching data:", error);
    }
};
</script>

{#if loading}
<div class="text-center mt-5 mb-0">
  <h1 class="text-slate-500">*PSA : Loading Times Might Be High Right Now. We Are Currently Running On Temp Servers.</h1>
</div>
<div class="mt-12 block text-center p-3 lg:flex lg:justify-center align-center content-center top-0">
  <!-- Shimmer effect for the image -->
  <Skeleton class=" z-0 ml-[38%] mr-[50%] lg:ml-0 lg:mr-0 sm:w-[25vw] sm:h-[40vw] w-[30vw] h-[50vw] mb-2  rounded-sm" />
  <div class="flex justify-center space-x-4 ml-5 mb-4">
    <div class="block">
      <Skeleton class="sm:h-7 lg:h-9 h-10 w-60 rounded" />
      <br>
      <Skeleton class="sm:h-7 lg:h-15 h-15 w-40 rounded" />
      <br>
      <Skeleton class="h-7 w-60 rounded" />
      <br>
      <Skeleton class="h-7 w-60 rounded" />
    </div>
  </div>

  <!-- Shimmer effect for the text -->
 
  <!-- Shimmer effect for the buttons -->
  
</div>
{:else if pi}
<Layout/>
<div 
  style="
    background-image: url(data:image/svg+xml;base64,alotofcodehere), 
      linear-gradient(to bottom, rgba(255, 255, 255, 0) 0%, rgba(18, 18, 18, 1) 70%);
    background-size: cover;
    background-position: center;
  " 
  class="absolute z-0 w-full h-[70vh]">
  <img src={pcover} class="opacity-15 object-cover w-full h-full" alt="Backdrop image">
</div>

<div class=" relative top-20  z-0 w-auto text-center mb-20 justify-center flex lg:block ">
 
  <div class=" mt-12 block text-center p-3 lg:flex lg:justify-center align-center content-center top-0 ">

       <img src={pimg} alt="" class=" z-0 ml-[38%] mr-[50%] lg:ml-0 lg:mr-0 sm:w-[25vw] sm:h-[40vw] w-[30vw] h-[50vw] mb-2  rounded-sm">
       
        <div class="block">
           {#if !dumbimgs}
            <h1 class="ml-5 text-3xl font-bold">{pi.title}</h1>
           {:else}
            <div class="flex justify-center">
            <img class=" w-[16vw] h-[8vw]" src={dumbimgs} alt="">
          </div>
          {/if} 
          <!-- > -->
          {#if type == "Tv Series"}
            <Button on:click={() => openplayer(showid, type)} class="m-5 w-max md:w-64">Watch From Ep 1</Button>
          {:else}
            <Button on:click={() => openplayer(showid, type)} class="m-5 w-max md:w-64">Watch</Button>
          {/if}
          <div class="flex justify-center p-3">
              <Badge class="ml-2" variant="secondary">{type}</Badge>
              <Badge class="ml-2" variant="secondary">{pi.vote_average}</Badge>
              <Badge class="ml-2" variant="secondary">{pi.runtime} mins</Badge>
              <Badge class="ml-2" variant="secondary">{pi.status}</Badge>
              <Badge class="ml-2" variant="secondary">{pi.release_date}</Badge>
          </div>
          <div class="ml-5 text-center mt-4 w-auto lg:w-72">
        <p>{pi.overview}</p>
        <div class="flex justify-center">
        <div class="flex p-2 overflow-x-scroll w-72">
        <!-- {#if pi && pi.actors}
          <p class="mr-2">Cast:</p>
          {#each pi.actors as cast}
            <Badge class="m-1 inline-block whitespace-nowrap">{cast}</Badge>
          {/each}
        {/if} -->
      </div>
      </div>
      <div class="flex p-2 justify-center w-72 overflow-x-scroll align-center ">
        {#if pi && pi.genres}
          <p class="mr-2">Genres:</p>
          {#each pi.genres as genre}
            <Badge class="m-1">{genre.name}</Badge>
          {/each}
        {/if}


      </div>
      </div>
      
        </div>
         <div class="content-right h-[47vw] overflow-scroll">
    {#if type == "Tv Series"}
        <div class="flex justify-center align-center">
          <DropdownMenu.Root class="outline-1 w-[400px]">
            <DropdownMenu.Trigger class="border border-gray-400 p-2 rounded-sm">Season - {allseasonsdata.season_number}</DropdownMenu.Trigger>
            <DropdownMenu.Content>
              <DropdownMenu.Group class="outline-2">
                <DropdownMenu.Label>Seasons</DropdownMenu.Label>
                <DropdownMenu.Separator />
                {#each pi.seasons as season}
                  <DropdownMenu.Item on:click={() => seasonchange(season.season_number)}>{season.season_number}</DropdownMenu.Item>
                {/each}
              </DropdownMenu.Group>
            </DropdownMenu.Content>
          </DropdownMenu.Root>
        </div>
        <br>
        <div class=" mr-5 ml-5 ">
          {#if eplist}
          {#each eplist as d}
            <button on:click={() => playnextep(showid, d.season_number, d.episode_number, type)}>
              <div class="flex pb-3 justify-center align-center sm:m-2 sm:flex sm:justify-center align-center ">
                <Card.Root>
                  <Card.Header>
                    <div class="block ml-auto mr-auto sm:w-auto sm:flex">
                      {#if d.still_path}
                      <img class="ml-auto mr-auto mb-3 sm:w-[220px] h-[120px] object-cover rounded" src={`https://image.tmdb.org/t/p/w500/${d.still_path}`} alt="">
                      {:else}
                      <img class="ml-auto mr-auto mb-3 sm:w-[220px] h-[120px] object-cover rounded" src={pcover} alt="">
                      {/if}
                      <div>
                        <Card.Title>{d.episode_number}. {d.name}</Card.Title>
                    <div class="flex mt-2 self-center justify-center align-center">
                      <CalendarDays class="h-[1.2rem] text-yellow-400 w-[1.2rem]" />
                      <p class="ml-1 text-sm mr-3 text-yellow-400">{d.air_date}</p>
                      <Tv class="h-[1.2rem] text-yellow-400 w-[1.2rem]" />
                      <p class="ml-1 text-sm mr-3 text-yellow-400">{d.runtime} mins</p>
                      </div>
                      <Card.Description class="ml-1">{d.overview}</Card.Description>
                    </div>
                      </div>
                  </Card.Header>
                </Card.Root>
              </div>
            </button>
          {/each}
          {:else}
          <h1>This season is currently not available</h1>
           {/if}
        </div>
      {/if}
  </div>
      </div>
  <div class="flex justify-center align-center">
    <div class="text-center">
      {#if !logoshow === "undefined"}
          <div class="flex align-center justify-center p-5">
            <img class="" src={logoshow} width="100px" height="100px" alt="">
          </div>
      {/if}
      <br>
      <br>
      

    </div>
  </div>
<br>
</div>
{/if}



<style>
  :global(body) {
  margin-top: 0px; /* Default margin for smaller screens */
  
  @media (min-width: 1024px) {
    margin-top: -50px; /* Override margin-top for larger screens */
  }
}

  ::-webkit-scrollbar {
  width: 6px;
  height: 4px;
}

/* Track */
::-webkit-scrollbar-track {
  background: transparent; 
}
 
/* Handle */
::-webkit-scrollbar-thumb {
  background: #3a3a3a; 
}

/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
  background: #555; 
}
</style>