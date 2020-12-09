<script lang="ts">
  import { onMount, onDestroy } from "svelte";
  
  import "../node_modules/xterm/css/xterm.css";
  import * as xterm from 'xterm';
  
  import * as fit from 'xterm-addon-fit';
  import { watchResize } from "svelte-watch-resize";
  
  let terminalElement;
  
  export let terminalController: xterm.Terminal;
  let termFit;
  
  export let theme: xterm.ITheme;
  export let bgColor : string;
  
  $: {
    bgColor = theme.background;
    terminalController.setOption('theme', theme);
  }
  
  function initializeXterm () {
    terminalController = new xterm.Terminal();
    
    termFit = new fit.FitAddon();
    terminalController.loadAddon(termFit);
    
    terminalController.open(terminalElement);
    termFit.fit();
  }
  
  onMount(async () => {
    initializeXterm();
  });
  
  function handleTermResize(node) {
    termFit.fit();
  }
  
</script>

<div id="terminal" bind:this={terminalElement} use:watchResize={handleTermResize}>
  
</div>

<style>
  :global(.terminal) {
    padding: 5px;
  }
  #terminal {
    margin: 0;
    height: 100%;
    width: 100%;
  }
</style>