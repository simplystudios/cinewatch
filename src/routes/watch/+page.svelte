<script>
// @ts-nocheck

import { onMount } from "svelte";

let id = "";
let se = "";
let serverid = "super1";
let type = "";
let frameurl = "";
let pageurl = "";

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

  type = matches[2];

  console.log(pageurl);

  updateFrameUrl();
});

function updateFrameUrl() {
  if (type === "TV%20Series") {
    se = se.replace("-", "/");
    console.log(se);
    if (serverid === "vidsrc") {
      frameurl = `https://vidsrc.me/embed/tv/${id}/${se}`;
    } else if (serverid === "vidsrc2") {
      frameurl = `https://vidsrc.me/embed/tv/${id}/${se}`;
    } else if (serverid === "vidsrc3") {
      frameurl = `https://vidsrc.pro/embed/tv/${id}/${se}`;
    } else if (serverid === "super1") {
      frameurl = `https://multiembed.mov/directstream.php?video_id=${id}&tmdb=1&s=${se}&e=2`;
    } else {
      frameurl = `https://lh6.googleusercontent.com/Bu-pRqU_tWZV7O3rJ5nV1P6NjqFnnAs8kVLC5VGz_Kf7ws0nDUXoGTc7pP87tyUCfu8VyXi0YviIm7CxAISDr2lJSwWwXQxxz98qxVfMcKTJfLPqbcfhn-QEeOowjrlwX1LYDFJN`;
    }
  } else {
    if (serverid === "vidsrc") {
      frameurl = `https://vidsrc.me/embed/movie/${id}`;
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

function handleServerChange(event) {
  serverid = event.target.value;
}
</script>

<div class="absolute  bg-slate-700 p-2 rounded-sm">
  <select on:change={handleServerChange} class=" bg-slate-700 text-white" name="server" id="server">
    <option value="vidsrc">Vidsrc</option>
    <option value="super1">Super1</option>
    <option value="vidsrc2">Vidsrc2</option>
    <option value="vidsrc3">Vidsrc3</option>
  </select>
  <button class=" text-white bg-slate-400 p-1 rounded-md" on:click={()=>updateFrameUrl()}>Choose</button>
</div>

<iframe src={frameurl} allowfullscreen width="100%" height="720px" frameborder="0"></iframe>
