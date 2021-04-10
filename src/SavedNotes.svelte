<script>
  export let savedNotes

  const saveNotes = () => {
    alert('Not available yet')
  }

</script>

<h1 class="subtitle">Saved</h1>
<button on:click={saveNotes} class="button is-dark">Save</button>
<hr />
{#each savedNotes as note}
   {note.timestamp}<br />
   {note.note} <br /><br />
{/each}