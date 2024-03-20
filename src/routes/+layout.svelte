<script>
    import "../app.pcss";
    import Separator from "$lib/components/ui/separator/separator.svelte";
    import { toggleMode } from "mode-watcher";
    import * as Dialog from "$lib/components/ui/dialog";
    import { Button } from "$lib/components/ui/button";
    import * as RadioGroup from "$lib/components/ui/radio-group";
    import Sun from "lucide-svelte/icons/sun";
    import * as Tabs from "$lib/components/ui/tabs";
    import { Settings } from 'lucide-svelte';
    import * as Alert from "$lib/components/ui/alert";
    import { Label } from "$lib/components/ui/label";
    import { Switch } from "$lib/components/ui/switch";
    import { TriangleAlert } from 'lucide-svelte';
    import Moon from "lucide-svelte/icons/moon";

    let dialogOpen = false;

    const toggled = () =>{
      dialogOpen = true;
      console.log("open")
    }
</script>
<slot >

  <div class="p-2 ">
        <h1 class="text-center text-2xl font-medium">CineWatch</h1>
         <p class=" text-center text-xs">🍿🎬</p>
    <div>
    </div>
    <div class="flex justify-center mt-2">
        <Button class="" on:click={toggleMode} variant="icon" size="icon">
              <Sun
                class="h-[1.2rem] w-[1.2rem] rotate-0 scale-100 transition-all dark:-rotate-90 dark:scale-0"
              />
              <Moon
                class="absolute h-[1.2rem] w-[1.2rem] rotate-90 scale-0 transition-all dark:rotate-0 dark:scale-100"
              />
              <span class="sr-only">Toggle theme</span>
        </Button>
        <Separator class=" w-11/12" orientation="vertical" />

        <Button class="" on:click={toggled} variant="icon" size="icon">
          <Settings
                class="absolute h-[1.2rem] w-[1.2rem] rotate-90 scale-100 transition-all dark:rotate-0 dark:scale-100"
              />
        </Button>
        </div>
    <Separator class="my-4" />
    <Dialog.Root bind:open={dialogOpen}>
      <Dialog.Trigger></Dialog.Trigger>
      <Dialog.Content>
        <Dialog.Header>
          <h1 class=" font-bold">Settings</h1>
          <Tabs.Root value="account" class=" w-auto">
  <Tabs.List class=" w-full">
    <Tabs.Trigger class="w-auto" value="account">Simple</Tabs.Trigger>
    <Tabs.Trigger class="w-auto" value="password">Advanced</Tabs.Trigger>
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
<Label>Made By Punchoneman</Label>
  </Tabs.Content>
  <Tabs.Content value="password">
    <div class="flex items-center space-x-2">
  <Switch id="airplane-mode" />
  <Label for="airplane-mode">Airplane Mode</Label>
</div>
  </Tabs.Content>
</Tabs.Root>
        </Dialog.Header>
      </Dialog.Content>
    </Dialog.Root>


</slot>
