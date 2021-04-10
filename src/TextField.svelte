<script>
  import { onMount } from 'svelte';
  export let box
  export let ref = null;

  onMount(() => {
    ref.focus(); 
  });   
</script>


<div class="field">
  <slot></slot>
  <textarea label="{box}" bind:value={box.note} bind:this={ref} />
  <br />
</div>