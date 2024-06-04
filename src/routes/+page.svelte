<script lang="ts">
  // @ts-nocheck
  
  import { Button } from "$lib/components/ui/button";
  import * as Card from "$lib/components/ui/card";
  import { TriangleAlert } from 'lucide-svelte';
  import Layout from "./+layout.svelte";
  import { Twitter } from 'lucide-svelte';
  import { setMode, resetMode, mode } from "mode-watcher";
  import { CalendarDays } from 'lucide-svelte';
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

  let searchterm = "";
  let searchd = [];
  let searchcss = "flex items-center justify-center p-40 pt-10 pb-20 mb-10";
  let cssp = "";
  let allseasons = { seasons: [] }; // Initialize with empty seasons array
  let dialogOpen = false;
  let hid = "flex justify-center pt-20";
  let hid2 = "flex justify-center";
  let epnum = 0;
  let showid
  let dialogPlayer = false;
  let serverd = [];
  let infoj = [];
  let eplist = [];
  let allseasonsdata = {};
  let playerurl;

  const ggs = async (data, type) => {
    dialogOpen = true;
    let bata = data;
    let pi;
    let api = `https://consumetmuyi.vercel.app/meta/tmdb/info/${bata}?type=${type}`;
    pi = await fetch(api);
    pi = await pi.json();
    if (type === "TV Series") {
      seasonsel(api);
    }
    infoj = pi;
  };

  const seasonsel = async (id) => {
    let apib = id;
    let response = await fetch(apib);
    let data = await response.json();
    console.log(data);
    allseasons = data;
    showid = allseasons.mappings.tmdb;
    console.log(allseasons)
    console.log(allseasons.seasons)
    console.log(allseasons.seasons[0])
    allseasonsdata = allseasons.seasons[0];
    eplist = allseasonsdata.episodes;
    console.log(eplist)
  };

  const seasonchange = (no) => {
    let seasono = no - 1;
    console.log(seasono)
    allseasonsdata = allseasons.seasons[seasono];
    console.log(allseasonsdata);
    eplist = allseasonsdata.episodes;
  };

  const playep = (id, season, ep) => {
    dialogOpen = false;
    epnum = ep;
    playerurl = `https://vidsrc.xyz/embed/${id}/${season}-${ep}`;
    console.log(playerurl)
    dialogPlayer = true;
  };

  const playnextep = (id, season, ep) => {
    dialogPlayer = false;
    epnum = ep;
    playerurl = `https://vidsrc.xyz/embed/${id}/${season}-${ep}/`;
    dialogPlayer = true;
  };

  const openplayer = async (id, type) => {
    if (type == "Tv Shows") {
      dialogOpen = false;
      dialogPlayer = true;
      playerurl = `https://vidsrc.xyz/embed/${id}/1-1/`;
      let test = await fetch(playerurl);
      test = await test.json();
      epnum = epnum + 1;
    } else {
      dialogOpen = false;
      dialogPlayer = true;
      playerurl = `https://vidsrc.xyz/embed/${id}`;
      let test = await fetch(playerurl);
      if (!test.ok) {
        playerurl = `https://multiembed.mov/?video_id=${id}`;
      }
    }
  };

  const func = async () => {
    try {
      const response = await fetch(`https://consumetmuyi.vercel.app/meta/tmdb/${searchterm}`);
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
  <div class={searchcss}>
    <Input class="w-96 h-14" on:change={func} bind:value={searchterm} placeholder="🔎 Search A Movie or a Show.."></Input>
  </div>
  <div class="grid md:grid-cols-5 gap-4 grid-cols-2">
    {#each searchd as d}
      <button on:click={() => ggs(d.id, d.type)}>
        <Card.Root class="border-gray-200 dark:border-zinc-700">
          <Card.Header class="p-3">
            <img class="object-cover rounded h-[300px] w-auto" src={d.image} alt="">
            <Card.Title>{d.title}</Card.Title>
            <Card.Description>{d.releaseDate} {d.type}</Card.Description>
          </Card.Header>
        </Card.Root>
      </button>
    {/each}
  </div>
</div>

<Dialog.Root bind:open={dialogPlayer}>
  <Dialog.Trigger class=""></Dialog.Trigger>
  <Dialog.Content class="overflow-y-auto overflow-x-hidden max-h-screen">
    <Dialog.Header class="">
      <iframe title="Movie" width="100%" height="300px" allowfullscreen src={playerurl} frameborder="0"></iframe>
      <Alert.Root class="mt-4">
        <TriangleAlert />
        <Alert.Title>Use Ad Blocker.</Alert.Title>
        <Alert.Description>
          We use freely available servers which can contain popup and ads. We strongly recommend you use an ad blocker.
        </Alert.Description>
      </Alert.Root>
      {#if infoj && infoj.seasons}
        <Button on:click={() => playnextep(showid, allseasonsdata.season, epnum + 1)} variant="outline">Next Episode</Button>
        <div class="grid-cols-3 p-2">
          <DropdownMenu.Root class="outline-1">
            <DropdownMenu.Trigger class="border border-gray-400 p-2 rounded-sm">Season - {allseasonsdata.season}</DropdownMenu.Trigger>
            <DropdownMenu.Content>
              <DropdownMenu.Group class="outline-2">
                <DropdownMenu.Label>Seasons</DropdownMenu.Label>
                <DropdownMenu.Separator />
                {#each infoj.seasons as season}
                  <DropdownMenu.Item on:click={() => seasonchange(season.season)}>{season.season}</DropdownMenu.Item>
                {/each}
              </DropdownMenu.Group>
            </DropdownMenu.Content>
          </DropdownMenu.Root>
        </div>
        <div class="h-14">
          {#each eplist as d}
            <button on:click={() => playnextep(showid, allseasonsdata.season, d.episode)}>
              <div class="flex p-2">
                <Card.Root>
                  <img class="w-full h-[100px] object-cover rounded-t-lg" src={d.img.hd} alt="">
                  <Card.Header>
                    <Card.Title>{d.title} (episode-{d.episode})</Card.Title>
                    <div class="flex mt-2">
                      <CalendarDays class="h-[1.2rem] text-yellow-400 w-[1.2rem]" />
                      <p class="ml-1 text-sm text-yellow-400">{d.releaseDate}</p>
                    </div>
                    <Card.Description>{d.description}</Card.Description>
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
  <Dialog.Trigger></Dialog.Trigger>
  <Dialog.Content class="overflow-y-auto max-h-screen">
    <Dialog.Header>
      <div class="flex">
        <img src={infoj.image} alt="" height="100px" width="100px" class="mb-2">
        <div class="block">
          <Dialog.Title class="ml-5">{infoj.title}</Dialog.Title>
          <Button on:click={() => openplayer(infoj.mappings.tmdb, infoj.contentType)} class="m-5 w-max md:w-64">Watch</Button>
          <Badge class="ml-5" variant="secondary">{infoj.type}</Badge>
          <Badge variant="secondary">{infoj.rating}</Badge>
          <Badge variant="secondary">{infoj.releaseDate}</Badge>
        </div>
      </div>
      <Dialog.Description>{infoj.description}</Dialog.Description>
      <div class="flex p-2 overflow-hidden overflow-x-scroll w-[440px]">
        {#if infoj && infoj.actors}
          <p class="mr-2">Cast:</p>
          {#each infoj.actors as cast}
            <Badge class="m-1">{cast}</Badge>
          {/each}
        {/if}
      </div>
      <div class="grid-cols-4 p-2">
        {#if infoj && infoj.genres}
          <p class="mr-2 overflow-x-hidden w-[200px]">Genres:</p>
          {#each infoj.genres as genre}
            <Badge class="m-1">{genre}</Badge>
          {/each}
        {/if}
      </div>
      {#if infoj && infoj.seasons}
        <div class="grid-cols-3 p-2">
          <DropdownMenu.Root class="outline-1">
            <DropdownMenu.Trigger class="border border-gray-400 p-2 rounded-sm">Season - {allseasonsdata.season}</DropdownMenu.Trigger>
            <DropdownMenu.Content>
              <DropdownMenu.Group class="outline-2">
                <DropdownMenu.Label>Seasons</DropdownMenu.Label>
                <DropdownMenu.Separator />
                {#each infoj.seasons as s}
                  <DropdownMenu.Item on:click={() => seasonchange(s.season)}>{s.season}</DropdownMenu.Item>
                {/each}
              </DropdownMenu.Group>
            </DropdownMenu.Content>
          </DropdownMenu.Root>
        </div>
        <div class="h-14">
          {#each eplist as d}
            <button on:click={() => playep(showid, allseasonsdata.season, d.episode)}>
              <div class="flex p-2">
                <Card.Root>
                  <img class="w-full h-[100px] object-cover rounded-t-lg" src={d.img.hd} alt="">
                  <Card.Header>
                    <Card.Title>{d.title} (episode-{d.episode})</Card.Title>
                    <div class="flex mt-2">
                      <CalendarDays class="h-[1.2rem] text-yellow-400 w-[1.2rem]" />
                      <p class="ml-1 text-sm text-yellow-400">{d.releaseDate}</p>
                    </div>
                    <Card.Description>{d.description}</Card.Description>
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

<Separator orientation="horizontal" />
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
