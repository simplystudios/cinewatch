<script>
// @ts-nocheck

    
    import { onMount } from "svelte";
    import { Pulse } from 'svelte-loading-spinners';
    import Layout from "./../+layout.svelte"
    import { Play } from 'lucide-svelte';
    import { Plus } from 'lucide-svelte';


    let netflixshows=[];
    let type = "movie"

 
    onMount(async()=>{
        const netflixResponse = await fetch(`https://api.themoviedb.org/3/movie/popular?api_key=07d7cff6553ffe45f88ee4c89a93a12c`);
        const netflixJson = await netflixResponse.json();
        netflixshows = netflixJson.results;
    })

    const ggs = async (data, type) => {
    window.open(`/info?id=${data}?type=${type}`, "_self");
  };

</script>

<Layout/>
<div class="absolute flex top-20 ml-5">
    <div class="px-4 md:px-12 space-y-8">
        <div class="flex">
            <h2 class="text-2xl font-bold ml-1">Top {type}</h2>
            <div>
                
            </div>
        </div>
  {#if netflixshows.length > 0}
  <div class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-5 gap-2">
    {#each netflixshows as d}
      <div on:click={() => ggs(d.id, "Movie")} class="group relative">  <img class="object-cover w-[210px] h-[320px] md:h-[320px] shadow-xl cursor-pointer transition duration group-hover:opacity-90 sm:group-hover:opacity-0 delay-100" src={`https://image.tmdb.org/t/p/w500/${d.poster_path}`} alt="">
        <div class="opacity-0 absolute top-0 transition duration-200 z-30 invisible sm:visible delay-300 w-full scale-0 group-hover:scale-110 group-hover:-translate-y-[6vw] group-hover:-translate-x-[1.4vw] group-hover:opacity-100">
          <img class="object-cover w-12/12 h-12/12 md:h-12/12  shadow-xl cursor-pointer transition rounded-t-md duration" src={`https://image.tmdb.org/t/p/w500/${d.poster_path}`} alt="">
          <div class="z-10 bg-zinc-800 p-2 lg:p-4 absolute w-[220px] transition shadow-md rounded-b-md">
            <div class="flex flex-row items-center gap-3">
              <button class="bg-white rounded-[100%] w-[30px] h-[30px] text-center">
                <Play color="black" size="16px" class="ml-[7px]"/>
              </button>
              <button class="bg-white rounded-[100%] w-[30px] h-[30px] text-center">
                <Plus color="black" size="16px" class="ml-[7px]"/>
              </button>
            </div>
            <h1 class="font-bold mb-1 line-clamp-1">{d.title}</h1>
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

</div>
<style>

</style>