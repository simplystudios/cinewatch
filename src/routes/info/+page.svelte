<script>
// @ts-nocheck

import { Button } from "$lib/components/ui/button";
  import * as Card from "$lib/components/ui/card";
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
let pi = [];
let eplist = [];
let allseasonsdata = {};
let playerurl;
let type;
let seasonid=0;
let pageurl = '';


 onMount(async() =>{
    pageurl=window.location.search;
    pageurl = pageurl.replace("?id=","")
    type = pageurl.replace("&type=","")
    console.log(pageurl);
    
     let api = `${baseurl}/meta/tmdb/info/${pageurl}`;
     pi = await fetch(api);
     pi = await pi.json();
     logoshow = pi.logos[0].url
     console.log(pi)
     if (pi.type === "TV Series") {
       allseasons = pi;
       showid = allseasons.mappings.tmdb;
       console.log(allseasons)
       console.log(allseasons.seasons)
       console.log(allseasons.seasons[0])
       allseasonsdata = allseasons.seasons[0];
       eplist = allseasonsdata.episodes;
       console.log(eplist)
   }
  })


const seasonchange = (no) => {
    let seasono = no - 1;
    console.log(seasono);
    allseasonsdata = allseasons.seasons[seasono];
    console.log(allseasonsdata);
    eplist = allseasonsdata.episodes;
};

const playep = (id, season, ep) => {
    window.open(`/watch?id=${id}?se=${season}-${ep}`);
};

const playnextep = (id, season, ep, type) => {
    window.open(`/watch?id=${id}?se=${season}-${ep}?type=${type}`);
};

const openplayer = async (id, type) => {
    if (type == "tv") {
        window.open(`/watch?id=${id}?se=${1-1}`);
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
{#if pi}
<Layout/>
<div style="background: url(data:image/svg+xml;base64,alotofcodehere);
    background: -moz-linear-gradient(top,   rgba(255,255,255,0) 0%, rgba(18,18,18,255) 70%);
    background: -webkit-gradient(linear, left top, left bottom, color-stop(0%,rgba(255,255,255,0)), color-stop(70%,#000000));
    background: -webkit-linear-gradient(top,  rgba(255,255,255,0) 0%, rgba(18,18,18,255) 70%);
    background: -o-linear-gradient(top,   rgba(255,255,255,0) 0%, rgba(18,18,18,255) 70%);
    background: -ms-linear-gradient(top,  rgba(255,255,255,0) 0%, rgba(18,18,18,255) 70%);
    background: linear-gradient(to bottom,  rgba(255,255,255,0) 0%, rgba(18,18,18,255) 70%);"  class="absolute z-0 h-[70vw]">
  <img src={pi.cover} class=" opacity-15 object-cover bg-black" alt="">
  </div>
<div class=" relative top-20  z-0 w-auto text-center mb-20 justify-center flex lg:block ">
 
  <div class=" mt-12 block text-center p-3 lg:flex lg:justify-center align-center content-center top-0 ">

       <img src={pi.image} alt="" class=" z-0 ml-[38%] mr-[50%] lg:ml-0 lg:mr-0 sm:w-[25vw] sm:h-[40vw] w-[30vw] h-[50vw] mb-2  rounded-sm">
       
        <div class="block">
           {#if !logoshow}
            <h1 class="ml-5 text-3xl font-bold">{pi.title}</h1>
           {:else}
            <div class="flex justify-center">
            <img class=" w-[16vw] h-[8vw]" src={logoshow} alt="">
          </div>
          {/if} 
          <!-- > -->
          <Button on:click={() => openplayer(pageurl, type)} class="m-5 w-max md:w-64">Watch</Button>
          <div class="flex justify-center p-3">
              <Badge class="ml-2" variant="secondary">{pi.type}</Badge>
              <Badge class="ml-2" variant="secondary">{pi.rating}</Badge>
              <Badge class="ml-2" variant="secondary">{pi.releaseDate}</Badge>
          </div>
          <div class="ml-5 text-center mt-4 w-auto lg:w-72">
        <p>{pi.description}</p>
        <div class="flex justify-center">
        <div class="flex p-2 overflow-x-scroll w-72">
        {#if pi && pi.actors}
          <p class="mr-2">Cast:</p>
          {#each pi.actors as cast}
            <Badge class="m-1 inline-block whitespace-nowrap">{cast}</Badge>
          {/each}
        {/if}
      </div>
      </div>
      <div class="flex p-2 justify-center w-72 overflow-scroll align-center ">
        {#if pi && pi.genres}
          <p class="mr-2">Genres:</p>
          {#each pi.genres as genre}
            <Badge class="m-1">{genre}</Badge>
          {/each}
        {/if}


      </div>
      </div>
      
        </div>
         <div class="content-right h-[47vw] overflow-scroll">
    {#if pi.type === "TV Series"}
        <div class="flex justify-center align-center">
          <DropdownMenu.Root class="outline-1 w-[400px]">
            <DropdownMenu.Trigger class="border border-gray-400 p-2 rounded-sm">Season - {allseasonsdata.season}</DropdownMenu.Trigger>
            <DropdownMenu.Content>
              <DropdownMenu.Group class="outline-2">
                <DropdownMenu.Label>Seasons</DropdownMenu.Label>
                <DropdownMenu.Separator />
                {#each pi.seasons as season}
                  <DropdownMenu.Item on:click={() => seasonchange(season.season)}>{season.season}</DropdownMenu.Item>
                {/each}
              </DropdownMenu.Group>
            </DropdownMenu.Content>
          </DropdownMenu.Root>
        </div>
        <br>
        <div class=" mr-5 ml-5 ">
          {#if eplist}
          {#each eplist as d}
            <button on:click={() => playnextep(showid, d.season, d.episode, pi.type)}>
              <div class="flex pb-3 justify-center align-center sm:m-2 sm:flex sm:justify-center align-center ">
                <Card.Root>
                  <Card.Header>
                    <div class="block ml-auto mr-auto sm:w-auto sm:flex">
                      {#if d.img}
                      <img class="ml-auto mr-auto mb-3 sm:w-[220px] h-[120px] object-cover rounded" src={d.img.hd} alt="">
                      {:else}
                      <img class="ml-auto mr-auto mb-3 sm:w-[220px] h-[120px] object-cover rounded" src={pi.cover} alt="">
                      {/if}
                      <div>
                        <Card.Title>{d.episode}. {d.title}</Card.Title>
                    <div class="flex mt-2 self-center justify-center align-center">
                      <CalendarDays class="h-[1.2rem] text-yellow-400 w-[1.2rem]" />
                      <p class="ml-1 text-sm mr-3 text-yellow-400">{d.releaseDate}</p>
                      <Tv class="h-[1.2rem] text-yellow-400 w-[1.2rem]" />
                      <p class="ml-1 text-sm mr-3 text-yellow-400">{d.season}</p>
                      </div>
                      <Card.Description class="ml-1">{d.description}</Card.Description>
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
{:else}
<div class="flex justify-center mt-[25%]">
  <div class="block ml-[50%] mr-[50%] w-[10vw]">
    <Pulse size="60" color="#FF3E00" unit="px" duration="1000s" />
    <h1 class="mt-2">Loading...</h1>
  </div>
</div>
<div class="block mt-[10%] text-center">
    <h1>If the site is loading for a while it might be a server issue</h1>
  </div>
{/if}



<style>
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