<script>
// @ts-nocheck

  import { onMount } from "svelte";
  import { Button } from "$lib/components/ui/button";
  import * as Card from "$lib/components/ui/card";
  import { Regex, TriangleAlert } from 'lucide-svelte';
  import Layout from "../+layout.svelte";
  import { page } from '$app/stores';
  import { Twitter } from 'lucide-svelte';
  import { setMode, resetMode, mode } from "mode-watcher";
  import { CalendarDays, Tv } from 'lucide-svelte';
  import { Coffee } from 'lucide-svelte';
  import * as Alert from "$lib/components/ui/alert";
  import Separator from "$lib/components/ui/separator/separator.svelte";
  import { Input } from "$lib/components/ui/input";
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
  let logoshow;
  let dialogOpen = false;
  let hid = "flex justify-center pt-20";
  let id;
  let se;
  let type;
  let hid2 = "flex justify-center";
  let epnum = 0;
  let showid
  let dialogPlayer = false;
  let serverd = [];
  let pi = [];
  let eplist = [];
  let allseasonsdata = {};
  let playerurl;
  let frameurl
  let pageurl = '';
  onMount(async() =>{
    pageurl=window.location.search;
    const regex = /[?&](id|se|type)=([^?&]+)/g;
    const matches = [];
    let match;

    while ((match = regex.exec(pageurl)) !== null) {
        matches.push(match[2]);
    }

    console.log(matches);
    id = matches[0]
    se = matches[1]
    type = matches[2]
    if(type === "TV%20Series"){
          se = se.replace("-","/");
    console.log(se)
        frameurl = `https://vidsrc.me/embed/tv/${id}/${se}`;
        console.log(frameurl);
    }
    else{
        frameurl = `https://vidsrc.me/embed/movie/${id}`;
        console.log(frameurl);
    }

    console.log(pageurl);

  })
</script>
<div class="flex p-10">
  <p>Server : </p>
  <select class="" name="server" id="server">
    <option value="volvo">Volvo</option>
    <option value="saab">Saab</option>
    <option value="opel">Opel</option>
    <option value="audi">Audi</option>
  </select>
</div>
<iframe src={frameurl} allowfullscreen width="100%" height="720px" frameborder="0"></iframe>