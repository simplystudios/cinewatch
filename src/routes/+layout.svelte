<script lang="js">
  import "../app.pcss";
  import Separator from "$lib/components/ui/separator/separator.svelte";
  import { toggleMode } from "mode-watcher";
  import { tmdbapikey } from "$lib/ss";
  import * as Dialog from "$lib/components/ui/dialog";
  import { Button } from "$lib/components/ui/button";
  import * as RadioGroup from "$lib/components/ui/radio-group";
  import Sun from "lucide-svelte/icons/sun";
  import { X } from "lucide-svelte";
  import Search from "lucide-svelte/icons/search";
  import * as Tabs from "$lib/components/ui/tabs";
  import { Settings } from "lucide-svelte";
  import * as Alert from "$lib/components/ui/alert";
  import { Label } from "$lib/components/ui/label";
  import { toast } from "svelte-sonner";
  import { setMode, resetMode, mode } from "mode-watcher";
  import { Switch } from "$lib/components/ui/switch";
  import { TriangleAlert } from "lucide-svelte";
  import Moon from "lucide-svelte/icons/moon";
  import { writable } from "svelte/store";
  import { onMount } from "svelte";

  // Search functionality
  let searchd = [];
  let searchterm = "";
  let dialogOpen = false;

  const ggs = async (id, type) => {
    // Map type to "Movie" and "Tv Series"
    const formattedType = type === "movie" ? "Movie" : type === "tv" ? "Tv Series" : type;
    window.open(`/info?id=${id}?type=${formattedType}`, "_self");
  };

  const func = async () => {
    if (!searchterm.trim()) return;

    try {
      const response = await fetch(
        `https://api.themoviedb.org/3/search/multi?api_key=${tmdbapikey}&query=${encodeURIComponent(searchterm)}`
      );
      const results = (await response.json()).results || [];

      // Filter results to only include movies and TV shows
      searchd = results.filter((item) => item.media_type === "movie" || item.media_type === "tv");
    } catch (error) {
      console.error("Error fetching search results:", error);
      toast.error("Failed to fetch search results. Please try again.");
    }
  };

  // Command Palette functionality
  const commands = [
    { label: "Home", action: () => (window.location.href = "/") },
    { label: "About Us", action: () => (window.location.href = "/about") },
    { label: "Contact", action: () => (window.location.href = "/contact") },
  ];

  let searchQuery = "";
  let filteredCommands = [];
  const isPaletteOpen = writable(false);

  const openPalette = () => isPaletteOpen.set(true);
  const closePalette = () => isPaletteOpen.set(false);

  const filterCommands = () => {
    filteredCommands = commands.filter((cmd) =>
      cmd.label.toLowerCase().includes(searchQuery.toLowerCase())
    );
  };

  // Keyboard shortcuts
  onMount(() => {
    document.addEventListener("keydown", (e) => {
      if (e.ctrlKey && e.key === "p") {
        e.preventDefault();
        openPalette();
      }
      if (e.key === "Escape") {
        closePalette();
      }
    });
  });
</script>

<slot>
  <!-- Top Search Bar -->
  <div class="fixed flex z-10 p-8 backdrop-blur-sm w-full bg-[#0e0d0d71] h-24">
    <Button on:click={toggleMode} class="" variant="link" size="icon">
        {#if $mode === "light"}
          <Sun class="h-[1.2rem] w-[1.2rem] text-foreground" />
        {:else}
          <Moon class="h-[1.2rem] w-[1.2rem] text-foreground" />
        {/if}
      </Button>
      <Separator class="mr-0" orientation="vertical" />
    <div class="flex h-10 z-50 w-full">
      <div class="flex border bg-[#0307126c] w-full  text-sm border-[#4D4A4A] rounded-md">
        <p on:click={openPalette} class="p-2  bg-transparent text-sm rounded-md w-full">Search with Ctrl + P</p>
        <Search size="18px" on:click={openPalette} color="#9AA0AD" class="hidden sm:block mt-[10px] mr-2" />
      </div>
    </div>
    <Separator class="" orientation="vertical" />
      <Button on:click={() => (dialogOpen = true)} variant="icon" size="icon">
        <Settings class="h-[1.2rem] w-[1.2rem]" />
      </Button>
      
  </div>

  {#if $isPaletteOpen}
    <div class="absolute inset-0 bg-black bg-opacity-60">
      <div class="fixed top-1/2 backdrop-blur-sm bg-[#0e0d0d71] w-96 z-20 left-1/2 transform -translate-x-1/2 -translate-y-1/2 bg-black rounded-lg shadow-lg max-h-72 overflow-y-auto p-4">
        <div on:click={() => closePalette()} class="flex space-x-full mb-2">
          <p style="font-size: 12px;">Press Esc to close</p>
          <X style="font-size: 12px; cursor:pointer; margin-left:65%; height:15px" />
        </div>
        <input
          type="text"
          bind:value={searchterm}
          on:change={func}
          placeholder="Search a Movie/TV..."
          class="w-full p-2 mb-4 border-b border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-400"
        />
        {#if searchd.length > 0}
          <div>
            {#each searchd as sd}
              <div
                on:click={() => ggs(sd.id, sd.media_type)}
                class="flex hover:cursor-pointer border mt-3 p-5 rounded-md border-[#4D4A4A] bg-[#030712c9]"
              >
                <img
                  class="h-[100px] mr-2"
                  src={sd.poster_path ? `https://image.tmdb.org/t/p/w1280/${sd.poster_path}` : '/fallback-image.png'}
                  alt={sd.title || sd.name}
                />
                <div class="p-5">
                  <h1>{sd.title || sd.name}</h1>
                  <p>{sd.release_date || sd.first_air_date}</p>
                  <!-- Map media_type to "Movie" or "TV Show" -->
                  <p>{sd.media_type === "movie" ? "Movie" : sd.media_type === "tv" ? "TV Show" : sd.media_type}</p>
                </div>
              </div>
            {/each}
          </div>
        {/if}
      </div>
    </div>
  {/if}
</slot>

<style>
  ::-webkit-scrollbar {
    width: 6px;
  }
  ::-webkit-scrollbar-track {
    background: transparent;
  }
  ::-webkit-scrollbar-thumb {
    background: #3a3a3a;
  }
  ::-webkit-scrollbar-thumb:hover {
    background: #555;
  }
</style>