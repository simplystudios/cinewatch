<script>
    import "../app.pcss";
    import Separator from "$lib/components/ui/separator/separator.svelte";
    import { toggleMode } from "mode-watcher";
    import * as Dialog from "$lib/components/ui/dialog";
    import { Button } from "$lib/components/ui/button";
    import * as RadioGroup from "$lib/components/ui/radio-group";
    import Sun from "lucide-svelte/icons/sun";
    import Search from "lucide-svelte/icons/search";
    import * as Tabs from "$lib/components/ui/tabs";
    import { Settings } from 'lucide-svelte';
    import * as Alert from "$lib/components/ui/alert";
    import { Label } from "$lib/components/ui/label";
    import { toast } from "svelte-sonner";
    import { setMode, resetMode, mode } from "mode-watcher";
    import { Switch } from "$lib/components/ui/switch";
    import { TriangleAlert } from 'lucide-svelte';
    import Moon from "lucide-svelte/icons/moon";
  import { onMount } from "svelte";
    let dialogOpen = false;
    let searchd=[];
    let searchterm = "";

    const toggled = () =>{
      dialogOpen = true;
      console.log("open")
    }
    const ggs = async (data, type) => {
    window.open(`/info?id=${data}?type=${type}`, "_self");
  };

    const func = async () => {
    const response = await fetch(`https://consumetmuyi.vercel.app/meta/tmdb/${searchterm}?limit=5`);
    console.log(searchterm);
    searchd = await response.json();
    searchd = searchd.results;
    console.log(searchd);
  };

    const homeback=() =>{
      
    }
    // onMount(()=>{

    // })
</script>
<slot >

  <div class=" absolute top-5 w-full">
        
    <div class="flex absolute w-[98%] ml-2 z-10 justify-center ">
        <Button on:click={toggleMode} variant="link" size="icon">
          {#if $mode === 'light'}
            <Sun
              class="h-[1.2rem] w-[1.2rem] rotate-0 scale-100 transition-all dark:-rotate-90 dark:scale-0 text-foreground"
            />
          {:else}
            <Moon
              class="h-[1.2rem] w-[1.2rem] rotate-90 scale-0 transition-all dark:rotate-0 dark:scale-100 text-foreground"
            />
          {/if}
        </Button>
        <Separator class=" w-4/12" orientation="vertical" />
        <div class="flex border bg-[#0307126c] w-full  text-sm border-[#4D4A4A] rounded-md">
          <input type="text" bind:value={searchterm} on:change={() => func()} placeholder="Search" class="p-5 w-full sm:w-full h-4  bg-transparent text-sm rounded-md ">
          <Search size="18px" on:click={() => func()} color="#9AA0AD" class="hidden sm:block mt-[10px] mr-2" />
        </div>
        <Separator class=" w-4/12" orientation="vertical" />

        <Button class="" on:click={toggled} variant="icon" size="icon">
          <Settings
                class="absolute h-[1.2rem] w-[1.2rem] rotate-90 scale-100 transition-all dark:rotate-0 dark:scale-100"
              />
        </Button>
      </div>
    {#if searchd.length>0}
      <div class="z-50 top-[50px] left-[0] sm:left-[35%] w-[100vw] sm:w-[30vw] h-[350px] overflow-scroll relative">
        {#each searchd as sd}
          <div onclick={ggs(sd.id,sd.type)} class="flex hover:cursor-pointer border mt-3 p-5 rounded-md border-[#4D4A4A] bg-[#030712c9]">
            <img class="h-[100px] mr-2" src={sd.image} alt="">
            <div class="p-5">
              <h1>{sd.title}</h1>
              <p>{sd.releaseDate}</p>
              <p>{sd.type}</p>
            </div>
          </div>
        {/each}
      </div>
    {/if}

    
    <Dialog.Root bind:open={dialogOpen}>
      <Dialog.Trigger></Dialog.Trigger>
      <Dialog.Content>
        <Dialog.Header>
          <h1 class=" font-bold">Settings</h1>
          <Tabs.Root value="account" class=" w-auto">
  <Tabs.List class=" w-full">
    <Tabs.Trigger class="w-auto" value="account">Simple</Tabs.Trigger>
    <Tabs.Trigger class="w-auto" value="password">About</Tabs.Trigger>
  </Tabs.List>
  <Alert.Root class="mt-4">
            <TriangleAlert class="" />
  <Alert.Title>Use Ad Blocker.</Alert.Title>
  <Alert.Description>
    We use freely available servers which can contain popup and ads. We Strongy recommend you using a Ad blocker. 
  </Alert.Description>
</Alert.Root>
  <Tabs.Content value="account">
    <div class="flex items-center space-x-2 mt-2 mb-3">
  <Switch id="Dark-mode" />
  <Label for="Dark-mode">Dark Mode</Label>
</div>
<Separator class="mb-1" orientation="horizontal" />

<Label>Layout</Label>
<RadioGroup.Root class="mt-2" value="option-one">
  <div class="flex items-center space-x-2">
    <RadioGroup.Item value="option-one" id="option-one" />
    <Label for="option-one">Grid</Label>
  </div>
  <div class="flex items-center space-x-2">
    <RadioGroup.Item value="option-two" id="option-two" />
    <Label for="option-two">List</Label>
  </div>
</RadioGroup.Root>
<br>
<Separator orientation="horizontal" />
<br>
<Label>Made By Ansh Wadhwa</Label>
  </Tabs.Content>
  <Tabs.Content value="password">
    <div class="items-center space-x-2">
  <Label class="mb-2">Disclaimer</Label>
  <p>CineWatch does not host any files, it merely links to 3rd party services. Legal issues should be taken up with the file hosts and providers. CineWatch is not responsible for any media files shown by the video providers.</p>
  <br>
  <Separator orientation="horizontal" />
  <br>
  <Label>Made By Ansh Wadhwa</Label>
</div>
  </Tabs.Content>
</Tabs.Root>
        </Dialog.Header>
      </Dialog.Content>
    </Dialog.Root>



</slot>


<style>
  ::-webkit-scrollbar {
  width: 6px;
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