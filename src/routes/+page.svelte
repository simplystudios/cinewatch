<script>
// @ts-nocheck

  import { Button } from "$lib/components/ui/button";
  import * as Card from "$lib/components/ui/card";
  import Layout from "./+layout.svelte";
  import { Star } from 'lucide-svelte';
  import { Input } from "$lib/components/ui/input";
  import * as DropdownMenu from "$lib/components/ui/dropdown-menu";
  import { Search } from 'lucide-svelte';
  import { Badge } from "$lib/components/ui/badge";
  import { Calendar } from 'lucide-svelte';
  import CardContent from "$lib/components/ui/card/card-content.svelte";
  import * as Dialog from "$lib/components/ui/dialog";
  import Warning from "postcss/lib/warning";
  import Root from "postcss/lib/root";

  let searchterm = "";
  // @ts-ignore
  /**
   * @type {string | any[]}
   */
  let searchd = [];
  let searchcss = " flex items-center justify-center p-40 mt-auto mb-auto"
  let dialogOpen = false;
  let epnum = 0;
  let dialogPlayer = false;
  let serverd = []
  let infoj = [];
  let eplist = [];
  let allseasonsdata = [];
  let playerurl;
  // @ts-ignore
  const ggs = async(data) =>{
    dialogOpen = true;
    let bata = data;
    let api = `https://imdb-api.new-anshwadhwa.workers.dev/title/${bata}`
    let pi = await fetch(api)
    // @ts-ignore
    pi = await pi.json()
    console.log(pi)
    // @ts-ignore
    infoj = pi;
    if(infoj && infoj.all_seasons){
      seasonsel(`/title/${infoj.id}/season/1`)
    }
  }

  const seasonsel = async(id) =>{
    console.log(id)
    let apib = "https://imdb-api.new-anshwadhwa.workers.dev"+id;
    let allseasons = await fetch(apib)
    allseasons = await allseasons.json()
    allseasonsdata = allseasons;
    eplist = allseasons.episodes;
    console.log(allseasonsdata);
  }

  const playep = () =>{
    
  }

  const playnextep = (id,season,ep) =>{
    dialogPlayer = false;
    playerurl = "https://multiembed.mov/?video_id="+id+ `&s=${season}&`+`e=${ep}`;
    dialogPlayer = true;
  }

  const openplayer = async(id,type) =>{
    if(type == "tvSeries"){
      dialogOpen = false;
      dialogPlayer = true;
      playerurl = "https://multiembed.mov/?video_id="+id+"&s=1&"+"e=1";
      let test = await fetch(playerurl)
      test = await test.json()
      epnum = epnum + 1;
      console.log(test);
    }
    else{
      dialogOpen = false;
      dialogPlayer = true;
      playerurl = "https://multiembed.mov/?video_id="+id
      let test = await fetch(playerurl)
      if (test.ok){
        console.log("working..")
      }
      else{
        playerurl = "https://vidsrc.to/embed/mv/"+id
        console.log(playerurl)
      }
    }
  }


  const func = async () => {
    try {
      const response = await fetch(`https://imdb-api.new-anshwadhwa.workers.dev/search?query=${searchterm}`);
      const searchData = await response.json();
      searchcss = "flex items-center justify-center p-10 mt-auto mb-aut0"
      searchd = searchData.results;
    } catch (error) {
      console.error("Error fetching data:", error);
    }
  };
</script>


<Layout/>
<div class="max-h-max">
  <div class={searchcss}>
    <Input class=" w-96 h-14" on:change={func} bind:value={searchterm} placeholder="🔎 Search A Movie or a Show.."></Input>
  </div>

  <div class="grid md:grid-cols-5 gap-4 grid-cols-2 ">
      {#each searchd as d}
      {#if searchd.length > 0}
      <button on:click={() => ggs(d.id)}>
          <Card.Root class=" border-gray-200 dark:border-zinc-700">
          <Card.Header class="p-3">
            <img class=" object-cover rounded h-[250px] w-auto" src={d.image} alt="">
            <Card.Title class="">
              {d.title}
            </Card.Title>
            <Card.Description>
              <div class="flex">
                <Calendar class="h-[1.2rem] w-[1.2rem] mr-1 scale-100 transition-all dark:- dark:scale-100" />
                {d.year}
              </div>
            </Card.Description>
          </Card.Header>
        </Card.Root>
      </button>
      
    {:else}
      console.warn("No Results")
    {/if}
    {/each}
    
  </div>
  
</div>

<Dialog.Root bind:open={dialogPlayer}>
  <Dialog.Trigger class=""></Dialog.Trigger>
  <Dialog.Content class="">
    <Dialog.Header class="">
      <iframe title="Movie" width="100%" height="300px" allowfullscreen=true  src={playerurl} frameborder="0"></iframe>
      {#if infoj && infoj.all_seasons}
        <Button on:click={() => playnextep(infoj.id,1,epnum+1)} variant="outline">Next Episode</Button>
      {/if}
    </Dialog.Header>
  </Dialog.Content>
</Dialog.Root>

<Dialog.Root bind:open={dialogOpen}>
  <Dialog.Trigger ></Dialog.Trigger>
  <Dialog.Content class="overflow-y-scroll max-h-screen" >
    <Dialog.Header>
      <div class="flex">
        <img src={infoj.image} alt="" height="100px" width="100px" class="mb-2">
        <div class="block">
          <Dialog.Title class=" ml-5">{infoj.title}</Dialog.Title>
          <Button on:click={() => openplayer(infoj.id,infoj.contentType)} class="m-5 w-max md:w-64 ">Watch</Button>
          <Badge class="ml-5" variant="secondary">{infoj.runtime}</Badge>
          <Badge variant="secondary">{infoj.contentRating}</Badge>
          <Badge variant="secondary">{infoj.year}</Badge>
          <!-- <Badge variant="secondary">{infoj.rating.star}</Badge> -->
        </div>
      </div>
      <Dialog.Description class=""> 
        {infoj.plot}
      </Dialog.Description>
      <div class="flex p-2">
        {#if infoj && infoj.actors}
  <p class=" mr-2">Cast:</p>
  {#each infoj.actors as cast}
    <Badge class="m-1">{cast}</Badge>
  {/each}
{/if}
      </div>
      <div class="grid-cols-4 p-2">
        {#if infoj && infoj.genre}
  <p class=" mr-2">Genres:</p>
  {#each infoj.genre as genre}
    <Badge class="m-1">{genre}</Badge>
  {/each}
{/if}
      </div>
{#if infoj && infoj.all_seasons}
      <div class="grid-cols-3 p-2">
         <DropdownMenu.Root class=" outline-1">
  <DropdownMenu.Trigger class="border border-gray-400 p-2 rounded-sm">Season - {allseasonsdata.season_id}</DropdownMenu.Trigger>
  <DropdownMenu.Content>
    <DropdownMenu.Group class="outline-2">
      <DropdownMenu.Label>Seasons</DropdownMenu.Label>
      <DropdownMenu.Separator />
      {#each infoj.all_seasons as season}
        <DropdownMenu.Item on:click={()=> seasonsel(season.api_path)}>{season.name}</DropdownMenu.Item>
      {/each}
    </DropdownMenu.Group>
  </DropdownMenu.Content>
</DropdownMenu.Root>
      </div>
      <div class="h-14">
        {#each eplist as d}
      <div class="flex p-2">
            <Card.Root>
              <img class=" w-full h-[100px] object-cover rounded-t-lg rounded-tr-" src={d.image} alt="">
          <Card.Header>
            <Card.Title>
              {d.title} (episode-{d.no})
              <div class="flex mt-2">
                <Star class="h-[1.2rem] text-yellow-400 w-[1.2rem] rotate-0 scale-100 transition-all" />
                <p class="ml-1 text-sm text-yellow-400">
                  {d.rating.star}
                </p>
              </div>
            </Card.Title>
            <Card.Description>
              {d.plot}
            </Card.Description>
          </Card.Header>
        </Card.Root>
      </div>
      {/each}
      </div>
      {/if}
    </Dialog.Header>
  </Dialog.Content>
</Dialog.Root>

<footer class=" text-center relative bottom-auto top-auto">
  <div>
    <p>Made by Ansh Wadhwa</p>
    <p>
      No File is Stored on our site all data is from freely available online services.
    </p>
  </div>
</footer>