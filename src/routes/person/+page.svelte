<script>
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
  import CardHeader from "$lib/components/ui/card/card-header.svelte";


  let pid = "";
  let pageurl = "";
  let pinfo = []
  let bio="";
  let pmv = [];
  let pimg = ""

    const ggs = async (data, type) => {
        if (type === "movie") {
            type = "Movie";
        } else if (type === "tv") {
            type = "Tv Series";
        }
    window.open(`/info?id=${data}?type=${type}`, "_self");
  };

  onMount(async () => {
     pageurl=window.location.search;
    pageurl = pageurl.replace(/\?(?=[^?]*$)/, "&");

// Parse the corrected query string
    const params = new URLSearchParams(pageurl);
    pid = params.get("id");
    const req = `https://api.themoviedb.org/3/person/${pid}?api_key=07d7cff6553ffe45f88ee4c89a93a12c`
    const req2 = `https://api.themoviedb.org/3/person/${pid}/combined_credits?api_key=07d7cff6553ffe45f88ee4c89a93a12c`
    const response = await fetch(req);
    const response2 = await fetch(req2);
    pinfo = await response.json();
    pmv = await response2.json();
    pmv = pmv.cast;
    bio= pinfo.biography;
    console.log(bio)
    console.log(pmv)
    console.log(pinfo);
    pimg = `https://image.tmdb.org/t/p/w600_and_h900_bestv2/${pinfo.profile_path}`
  });

</script>

<div class=" relative md:top-28 top-20 z-0 w-auto text-center mb-20 justify-center flex lg:block ">
 
  <div class=" mt-12 block text-center p-3 lg:flex lg:justify-center align-center content-center top-0 ">
      {#if pimg}
      <div class="flex justify-center">
        <div class="aspect-[2/3] w-[342px] lg:w-[380px]">
          <img 
            src={pimg} 
            alt="Movie Poster" 
            class="object-cover w-full h-full rounded-sm z-0 mb-2"
          >
        </div>
      </div>
      {:else}
      <img src="/stranger.jpg" alt="" class=" z-0 ml-[38%] mr-[50%] lg:ml-0 lg:mr-0 sm:w-[25vw] sm:h-[40vw] w-[30vw] h-[50vw] mb-2  rounded-sm">
      {/if}
      <div class="block">
          <h1 class="ml-5 text-3xl font-bold">{pinfo.name}</h1>
        <!-- > -->
        
        <div class="flex justify-center p-3">
          <Badge class="ml-2" variant="secondary">{pinfo.birthday}</Badge>
        </div>
        <div class="ml-5 text-center mt-4 w-auto lg:w-72">
         <p class="">{bio}</p>
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
                
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="px-4 md:px-12 mt-20 space-y-8 z-0 mb-10">
    <h2 class="text-2xl font-bold ml-1 mb-5 z-0">Known For</h2>
    {#if pmv.length>0}
    <div class="grid h-full grid-cols-2 sm:grid-cols-3 md:grid-cols-5 ">
      {#each pmv as d}
        <div on:click={() => ggs(d.id, d.media_type)} class="group z-100">
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


<style>

</style>