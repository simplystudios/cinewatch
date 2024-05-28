<script>
// @ts-nocheck
  
  import { Button } from "$lib/components/ui/button";
  import * as Card from "$lib/components/ui/card";
  import { TriangleAlert } from 'lucide-svelte';
  import Layout from "./+layout.svelte";
  import { Twitter } from 'lucide-svelte';
  import { setMode, resetMode, mode } from "mode-watcher";
  import { Star } from 'lucide-svelte';
  import { Coffee } from 'lucide-svelte';
  import * as Alert from "$lib/components/ui/alert";
  import Separator from "$lib/components/ui/separator/separator.svelte";
  import { Input } from "$lib/components/ui/input";
  import * as DropdownMenu from "$lib/components/ui/dropdown-menu";
  import { Github } from 'lucide-svelte';
  import { Badge } from "$lib/components/ui/badge";
  import * as Accordion from "$lib/components/ui/accordion";
  import * as Dialog from "$lib/components/ui/dialog";
  import Warning from "postcss/lib/warning";
  import Root from "postcss/lib/root";

  let searchterm = "";
  // @ts-ignore
  /**
   * @type {string | any[]}
   */
  let searchd = [];
  let searchcss = " flex items-center justify-center p-40 pt-10 pb-20 mb-10"
  let cssp = ""
  let dialogOpen = false;
  let hid = "flex justify-center pt-20"
  let hid2 = "flex justify-center"
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

  const playep = (id,season,ep) =>{
    dialogOpen = false;
    epnum = ep;
    playerurl = "https://vidsrc.xyz/embed/"+id+ `/${season}`+`-${ep}`;
    dialogPlayer = true;
  }

  const playnextep = (id,season,ep) =>{
    dialogPlayer = false;
    epnum = ep;
    playerurl = "https://vidsrc.xyz/embed/"+id+ `/${season}`+`-${ep}/`;
    dialogPlayer = true;
  }

  const openplayer = async(id,type) =>{
    if(type == "tvSeries"){
      dialogOpen = false;
      dialogPlayer = true;
      playerurl = "https://vidsrc.xyz/embed/"+id+"/1"+"-1/";
      let test = await fetch(playerurl)
      test = await test.json()
      epnum = epnum + 1;
      console.log(test);
    }
    else{
      dialogOpen = false;
      dialogPlayer = true;
      playerurl = "https://vidsrc.xyz/embed/"+id
      let test = await fetch(playerurl)
      if (test.ok){
        console.log("working..")
      }
      else{
        playerurl = "https://multiembed.mov/?video_id="+id
        console.log(playerurl)
      }
    }
  }


  const func = async () => {
    try {
      const response = await fetch(`https://imdb-api.new-anshwadhwa.workers.dev/search?query=${searchterm}`);
      const searchData = await response.json();
      searchcss = "flex items-center justify-center pb-10 pt-4 mb-auto"
      cssp = "hidden"
      hid = "hidden"
      hid2 = "hidden"
      searchd = searchData.results;
    } catch (error) {
      console.error("Error fetching data:", error);
    }
  };
</script>


<Layout/>


<div class="max-h-max">
  <div class={hid}>
    {#if $mode == "dark"}
      <img src="/logo23.png" alt="" height="100" width="100">
    {:else if $mode == "light"}
      <img src="/logo23black.png" alt="" height="100" width="100">
    {:else}
      <img src="/logo23.png" alt="" height="100" width="100">
    {/if}
  </div>
  <div class={hid2}>
          <button variant="link" class="text-center text-2xl font-medium mt-4">CineWatch</button>
        </div>
    <div>
  </div>
  <div class={searchcss}>
    <Input class=" w-96 h-14" on:change={func} bind:value={searchterm} placeholder="🔎 Search A Movie or a Show.."></Input>
  </div>
<div>


</div>
  <div class="grid md:grid-cols-5 gap-4 grid-cols-2 ">
      {#each searchd as d}
      {#if searchd.length > 0}
      <button on:click={() => ggs(d.id)}>
          <Card.Root class=" border-gray-200 dark:border-zinc-700">
          <Card.Header class="p-3">
            <img class=" object-cover rounded h-[300px] w-auto" src={d.image} alt="">
            <Card.Title class="">
              {d.title}
            </Card.Title>
            <Card.Description>
                {d.year}
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
  <Dialog.Content class="overflow-y-scroll max-h-screen">
    <Dialog.Header class="">
      <iframe title="Movie" width="100%" height="300px" allowfullscreen=true  src={playerurl} frameborder="0"></iframe>
      <Alert.Root class="mt-4">
            <TriangleAlert class="" />
  <Alert.Title>Use Ad Blocker.</Alert.Title>
  <Alert.Description>
    We use freely available servers which can contain popup and ads. We Strongy recommend you using a Ad blocker. 
  </Alert.Description>
</Alert.Root>
      {#if infoj && infoj.all_seasons}
        <Button on:click={() => playnextep(allseasonsdata.id,allseasonsdata.season_id,epnum+1)} variant="outline">Next Episode</Button>
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
        <button on:click={playnextep(allseasonsdata.id,allseasonsdata.season_id,d.idx)}>
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
        </button>
      {/each}
      </div>
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
          {#if infoj.id && infoj.contentType}
            <Button on:click={() => openplayer(infoj.id,infoj.contentType)} class="m-5 w-max md:w-64 ">Watch</Button>
          {/if}
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
        <button on:click={playep(allseasonsdata.id,allseasonsdata.season_id,d.idx)}>
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
        </button>
      {/each}
      </div>
      {/if}
    </Dialog.Header>
  </Dialog.Content>
</Dialog.Root>
<Separator class="" orientation="horizontal" />
<footer class="h-max bottom-auto top-auto mb-3">
  <div class="flex mt-5 pr-8 justify-between">
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
            <Github/>
          </a>
          <!-- <Mail href="" /> -->

    </div>
    </div>
    
    <div class=" text-right bottom-0 left-0 float-right ml-10">
      <h1 class=" font-bold mb-1">About</h1>
       <p class=" text-right bottom-0 left-0 float-right ml-10 ">
      Made By Ansh Wadhwa
    </p>
    </div>
  </div>
</footer>