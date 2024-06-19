<script>
// @ts-nocheck

import { onMount } from "svelte";
import { Button } from "$lib/components/ui/button";
  import * as Card from "$lib/components/ui/card";
  import { TriangleAlert } from 'lucide-svelte';
  import { Twitter } from 'lucide-svelte';
  import { setMode, resetMode, mode } from "mode-watcher";
  import { CalendarDays } from 'lucide-svelte';
  import { Coffee } from 'lucide-svelte';
  import * as Alert from "$lib/components/ui/alert";
   import Layout from "./../+layout.svelte";
  import Separator from "$lib/components/ui/separator/separator.svelte";
  import { Input } from "$lib/components/ui/input";
  import * as DropdownMenu from "$lib/components/ui/dropdown-menu";
  import { Github } from 'lucide-svelte';
  import { Badge } from "$lib/components/ui/badge";
  import * as Accordion from "$lib/components/ui/accordion";
  import * as Dialog from "$lib/components/ui/dialog";
  import { Calendar } from "bits-ui";

let id = "";
let se = "";
let serverid = "super1";
let type = "";
let frameurl = "";
let pageurl = "";
let showid = "";
let dialogPlayer = false;
let episodeNumber=1;
 let allseasons = { seasons: [] }; // Initialize with empty seasons array
let eplist = [];
let infoj = [];
let seasonNumber = 1;
let allseasonsdata = {};

onMount(() => {
  pageurl = window.location.search;
  const regex = /[?&](id|se|type)=([^?&]+)/g;

  const matches = [];
  let match;

  while ((match = regex.exec(pageurl)) !== null) {
    matches.push(match[2]);
  }

  console.log(matches);
  id = matches[0];
  se = matches[1];
  [seasonNumber,episodeNumber] = se.split("-");
  console.log(episodeNumber,seasonNumber)

  type = matches[2];

  console.log(pageurl);

  updateFrameUrl();
});

 const seasonsel = async () => {
    let apib = `https://consumetmuyi.vercel.app/meta/tmdb/info/${id}?type=${type}`;
    dialogPlayer = true;
    let response = await fetch(apib);
    let data = await response.json();
    console.log(data);
    allseasons = data;
    showid = id;
    console.log(allseasons)
    console.log(allseasons.seasons)
    console.log(allseasons.seasons[0])
    allseasonsdata = allseasons.seasons[0];
    eplist = allseasonsdata.episodes;
    console.log(eplist)
  };


  const playep = (id, season, ep) => {
    window.open(`/watch?id=${id}?se=${season}-${ep}&type=${type}`,"_self")
    // dialogOpen = false;
    // epnum = ep;
    // playerurl = `https://vidsrc.xyz/embed/${id}/${season}-${ep}`;
    // console.log(playerurl)
    // dialogPlayer = true;
  };

function changep(){
  episodeNumber++
  window.open(`/watch?id=${id}?se=${seasonNumber}-${episodeNumber}?type=${type}`,"_self")
}

function updateFrameUrl() {
  if (type === "TV%20Series") {
    se = se.replace("-", "/");
    console.log(se);
    if (serverid === "smashybro") {
      frameurl = `https://player.smashy.stream/tv/${id}?s=${seasonNumber}&e=${episodeNumber}`;
    } else if (serverid === "vidsrc2") {
      frameurl = `https://vidsrc.me/embed/tv/${id}/${se}`;
    } else if (serverid === "vidsrc3") {
      frameurl = `https://vidsrc.pro/embed/tv/${id}/${se}`;
    } else if (serverid === "super1") {
      frameurl = `https://multiembed.mov/directstream.php?video_id=${id}&tmdb=1&s=${seasonNumber}&e=${episodeNumber}`;
    } else {
      frameurl = `https://lh6.googleusercontent.com/Bu-pRqU_tWZV7O3rJ5nV1P6NjqFnnAs8kVLC5VGz_Kf7ws0nDUXoGTc7pP87tyUCfu8VyXi0YviIm7CxAISDr2lJSwWwXQxxz98qxVfMcKTJfLPqbcfhn-QEeOowjrlwX1LYDFJN`;
    }
  } else {
    if (serverid === "smashybro") {
      frameurl = `https://player.smashy.stream/movie/${id}`;
    } else if (serverid === "vidsrc2") {
      frameurl = `https://vidsrc.me/embed/movie/${id}`;
    } else if (serverid === "vidsrc3") {
      frameurl = `https://vidsrc.pro/embed/movie/${id}`;
    } else if (serverid === "super1") {
      frameurl = `https://multiembed.mov/directstream.php?video_id=${id}&tmdb=1`;
    } else {
      frameurl = `https://lh6.googleusercontent.com/Bu-pRqU_tWZV7O3rJ5nV1P6NjqFnnAs8kVLC5VGz_Kf7ws0nDUXoGTc7pP87tyUCfu8VyXi0YviIm7CxAISDr2lJSwWwXQxxz98qxVfMcKTJfLPqbcfhn-QEeOowjrlwX1LYDFJN`;
    }
  }
  console.log(frameurl);
}

  const seasonchange = (no) => {
    let seasono = no - 1;
    console.log(seasono)
    allseasonsdata = allseasons.seasons[seasono];
    console.log(allseasonsdata);
    eplist = allseasonsdata.episodes;
  };


function handleServerChange(event) {
  serverid = event.target.value;
}
</script>

<div class="absolute  bg-slate-700 p-2 rounded-sm">
  <select on:change={handleServerChange} class=" bg-slate-700 text-white" name="server" id="server">
    <option value="super1">Super1</option>
    <option value="vidsrc2">Vidsrc2</option>
    <option value="smashybro">Smashybro</option>
    <option value="vidsrc3">Vidsrc3</option>
  </select>
  <button class=" text-white bg-slate-400 p-1 rounded-md" on:click={()=>updateFrameUrl()}>Choose</button>
  {#if type==="TV%20Series"}
  <button class=" text-white bg-slate-400 p-1 rounded-md" on:click={()=>changep()}>Next Episode</button>
  <button class=" text-white bg-slate-400 p-1 rounded-md" on:click={()=>seasonsel()}>Show All Episode</button>
  {/if}
</div>

<iframe src={frameurl} allowfullscreen width="100%" height="720px" frameborder="0"></iframe>

<Dialog.Root bind:open={dialogPlayer}>
  <Dialog.Trigger class=""></Dialog.Trigger>
  <Dialog.Content class="overflow-y-auto overflow-x-hidden max-h-screen">
    <Dialog.Header class="mb-[50%]">
      <Alert.Root class="mt-4">
        <TriangleAlert />
        <Alert.Title>Use Ad Blocker.</Alert.Title>
        <Alert.Description>
          We use freely available servers which can contain popup and ads. We strongly recommend you use an ad blocker.
        </Alert.Description>
      </Alert.Root>
      {#if allseasons && allseasons.seasons}
        <div class="grid-cols-3 p-2">
          <DropdownMenu.Root class="outline-1">
            <DropdownMenu.Trigger class="border border-gray-400 p-2 rounded-sm">Season - {allseasonsdata.season}</DropdownMenu.Trigger>
            <DropdownMenu.Content>
              <DropdownMenu.Group class="outline-2">
                <DropdownMenu.Label>Seasons</DropdownMenu.Label>
                <DropdownMenu.Separator />
                {#each allseasons.seasons as season}
                  <DropdownMenu.Item on:click={() => seasonchange(season.season)}>{season.season}</DropdownMenu.Item>
                {/each}
              </DropdownMenu.Group>
            </DropdownMenu.Content>
          </DropdownMenu.Root>
        </div>
        <div class="h-14">
          {#each eplist as d}
            <button on:click={() => playep(showid, d.season, d.episode)}>
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
