<script lang="ts" context="module">
  export interface BatData {
    name: null | string;
    isHindi: boolean;
  }
</script>

<script lang="ts">
  import { onMount } from "svelte";
  import EnterYourName from "./lib/EnterYourName.svelte";
  import BatCanvas from "./lib/BatCanvas.svelte";
  import Disclaimer from "./lib/Disclaimer.svelte";

  let nameRef: any;

  let batData: BatData = {
    name: null,
    isHindi: false,
  };

  let nameDialogOpen: boolean = true;

  onMount(() => {
    nameRef.focus();
  });

  const isHindi = (name: string) => {
    for (let c of name) {
      if (/^[\u0900-\u097F]+$/.test(c)) {
        return true;
      }
    }
    return false;
  };

  const nameSubmit = (event: { detail: string }) => {
    batData = {
      name: event.detail.toUpperCase(),
      isHindi: isHindi(event.detail),
    };
    console.log(batData);
    nameDialogOpen = false;
  };
</script>

<main
  class="min-h-screen bg-black text-white/80 flex flex-col items-center justify-center p-4 space-y-4 selection:text-orange-100 selection:bg-orange-600"
>
  <BatCanvas
    {batData}
    on:edit={() => {
      nameDialogOpen = true;
    }}
    class="w-full md:max-w-lg lg:max-w-xl rounded-lg ring-2 ring-orange-800/50"
  />
  <Disclaimer />

  {#if nameDialogOpen}
    <EnterYourName
      name={batData.name}
      on:submit={nameSubmit}
      bind:ref={nameRef}
    />
  {/if}
</main>
