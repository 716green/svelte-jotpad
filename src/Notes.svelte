<script>
  import moment from 'moment';
  import TextField from './TextField.svelte';
  import SavedNotes from './SavedNotes.svelte';

  export let title;

  let initialTimestamp = moment().format('YYYY-MM-DD h:mm a');
  let note = '';
  let jotboxes = [{ timestamp: initialTimestamp, note: note }];

  let savedNotes = [];

  export let showSaved = true;
  export let saveDisplay = 'hide';

  const toggleShowSaved = () => {
    showSaved = !showSaved;
    saveDisplay = saveDisplay === 'show' ? 'hide' : 'show';
  };

  const createTimestamp = () => {
    let stamp = moment().format('YYYY-MM-DD h:mm a');
    return stamp;
  };

  const createBox = () => {
    let stamp = createTimestamp();
    jotboxes = [...jotboxes, { timestamp: stamp, note: '' }];
  };

  const saveNote = () => {
    if (jotboxes[jotboxes.length - 1].note.trim().length < 1) {
      return;
    } else keysHeld = [];
    savedNotes = [
      ...savedNotes,
      { timestamp: jotboxes[jotboxes.length - 1].timestamp, note: jotboxes[jotboxes.length - 1].note },
    ];
    createBox();
    note = '';
  };

  export let keysHeld = [];

  const handleKeyup = () => {
    keysHeld = [];
  };

  const handleKeydown = (event) => {
    if (keysHeld.length === 0 || (keysHeld[0] === 'Meta' && keysHeld.length < 2)) {
      if (event.key === 'Meta' && keysHeld.length === 0) {
        keysHeld = [...keysHeld, 'Meta'];
      } else if (event.key === 'Enter' && keysHeld.length === 1) {
        keysHeld = [...keysHeld, 'Enter'];
        saveNote();
      } else {
        keysHeld = [];
      }
    } else {
      keysHeld = [];
    }
  };
</script>

<svelte:window on:keydown={(e) => handleKeydown(e)} on:keyup={() => handleKeyup()} />

<main>
  <div class="columns">
    <div class="column">
      <h1 class="subtitle">{title}</h1>

      <section>
        <button class="button is-white" disabled>Press <code>CTRL/CMD + Enter</code> to save</button>
        <hr />
        {#each jotboxes as box}
          <div class="jotbox-wrap">
            <div class="jotbox">
              <TextField {box}>
                <h4 class="jotbox-label">{box.timestamp}</h4>
              </TextField>
            </div>
          </div>
        {/each}
      </section>
    </div>
    {#if showSaved}
      <div class="column">
        <SavedNotes {savedNotes} />
      </div>
    {/if}
  </div>
  <button class="is-small button is-black" on:click={toggleShowSaved}>{saveDisplay} saved notes</button>
</main>
