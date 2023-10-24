<script>
  let notes = [];
  let newNote = {
    id: 1,
    x: 50,
    y: 50,
    header: 'Sticky Note',
    text: 'Edit me!',
    backgroundColor: 'yellow',
    isDragging: false,
  };

  function addStickyNote() {
    const id = notes.length > 0 ? notes[notes.length - 1].id + 1 : 1;
    notes = [...notes, { ...newNote, id }];
  }

  function handleNoteMove(index, event) {
    if (notes[index].isDragging) {
      notes[index].x = event.clientX - notes[index].dragStartX;
      notes[index].y = event.clientY - notes[index].dragStartY;
    }
  }

  function startDragging(index, event) {
    notes[index].isDragging = true;
    notes[index].dragStartX = event.clientX - notes[index].x;
    notes[index].dragStartY = event.clientY - notes[index].y;
  }

  function stopDragging(index) {
    notes[index].isDragging = false;
  }

  function deleteNote(index) {
    notes = notes.filter((note) => note.id !== notes[index].id);
  }

  function changeNoteColor(index, color) {
    notes[index].backgroundColor = color;
  }

  function changeNoteHeader(index, newHeader) {
    notes[index].header = newHeader;
  }
</script>

<main>
  <h1>Welcome to Sticky Notes</h1>
  <button on:click={addStickyNote}>Add Sticky Note</button>

  {#each notes as note, index (note.id)}
    <div
      class="custom-sticky-note"
      style="left: {note.x}px; top: {note.y}px; background-color: {note.backgroundColor};"
      on:mousemove={event => handleNoteMove(index, event)}
      role="button"
      aria-grabbed="{note.isDragging}"
      tabindex="0"
    >
      <div class="sticky-header" on:mousedown={event => startDragging(index, event)}>
        <input
          type="text"
          bind:value={note.header}
          placeholder="Sticky Note"
          class="header-input"
          on:input={() => changeNoteHeader(index, note.header)}
        />
      </div>
      <textarea bind:value={note.text}></textarea>
      <input
        type="color"
        bind:value={note.backgroundColor}
        class="color-picker"
        on:input={() => changeNoteColor(index, note.backgroundColor)}
      />
      <button on:click={() => deleteNote(index)} class="delete-button">Delete</button>
    </div>
  {/each}
</main>

<style>
  .custom-sticky-note {
    position: absolute;
    width: 250px;
    height: 200px;
    border: 2px solid #ccc;
    padding: 0;
    margin: 0;
  }

  .sticky-header {
    background-color: #ccc;
    padding: 8px;
    cursor: move;
  }

  .header-input {
    border: none;
    background: none;
    font-size: 16px;
    font-family: Arial, sans-serif;
  }

  textarea {
    width: calc(100% - 16px);
    height: calc(100% - 56px); /* Leave space for header, adjustments, and buttons */
    border: none;
    background: none;
    resize: none;
    margin: 8px;
  }

  .color-picker {
    position: absolute;
    bottom: 10px;
    left: 10px;
  }

  .delete-button {
    position: absolute;
    bottom: 10px;
    right: 10px;
    background-color: #ff6666;
    color: white;
    border: none;
    padding: 6px 12px;
    font-size: 14px;
    cursor: pointer;
    border-radius: 4px;
  }
</style>