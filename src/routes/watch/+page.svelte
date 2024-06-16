<script>
// @ts-nocheck

  import { onMount } from "svelte";

  let id="";
  let se="";
  let serverid="super1";
  let type="";
  let frameurl="";
  let pageurl = "";

  onMount(()=>{
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

    console.log(pageurl);
  });
  if(type === "TV%20Series"){
          se = se.replace("-","/");
    console.log(se)
        frameurl = `https://vidsrc.me/embed/tv/${id}/${se}`;
        console.log(frameurl);
    }
    else{
      if(serverid === "vidsrc"){
        frameurl = `https://vidsrc.me/embed/movie/${id}`;
        console.log(frameurl);
      }
      else if(serverid === "vidsrc2"){
        frameurl = `https://vidsrc.me/embed/movie/${id}`;
        console.log(frameurl);
      }
      else if(serverid === "vidsrc3"){
        frameurl = `https://vidsrc.pro/embed/movie/${id}`;
        console.log(frameurl);
      }
      else if(serverid === "super1"){
        frameurl = `https://multiembed.mov/directstream.php?video_id=${id}&tmdb=1`;
        console.log(frameurl);
      }
      else{
        frameurl= `https://lh6.googleusercontent.com/Bu-pRqU_tWZV7O3rJ5nV1P6NjqFnnAs8kVLC5VGz_Kf7ws0nDUXoGTc7pP87tyUCfu8VyXi0YviIm7CxAISDr2lJSwWwXQxxz98qxVfMcKTJfLPqbcfhn-QEeOowjrlwX1LYDFJN`
      }
    }
</script>
<div class="flex p-10">
  <p>Server : </p>
  <select on:select={serverid} class="" name="server" id="server">
    <option value="volvo">Vidsrc</option>
    <option value="saab">Super1</option>
    <option value="opel">Vidsrc2</option>
    <option value="audi">Vidsrc3</option>
  </select>
</div>
<iframe src={frameurl} allowfullscreen width="100%" height="720px" frameborder="0"></iframe>