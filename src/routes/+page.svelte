<!-- App.svelte -->
<script>
  let notes = [];
  let newNote = {
    id: 1,
    x: 50,
    y: 50,
    header: 'Sticky Note',
    text: 'Your Notes!',
    backgroundColor: '#D5CEA3',
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
    // Check if the left mouse button is pressed (button code 0)
    if (event.buttons === 1) {
      notes[index].isDragging = true;
      notes[index].dragStartX = event.clientX - notes[index].x;
      notes[index].dragStartY = event.clientY - notes[index].y;
    }
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
  <h1>Welcome to Sticky Notes 1.0 &#128515;</h1>
  <button on:click={addStickyNote}>Add Your Note</button>

  {#each notes as note, index (note.id)}
    <div
      class="custom-sticky-note"
      style="left: {note.x}px; top: {note.y}px; background-color: {note.backgroundColor};"
      on:mousemove={event => handleNoteMove(index, event)}
      role="button"
      aria-grabbed="{note.isDragging}"
      tabindex="0"
    >
      <div class="sticky-header" on:mousedown={event => startDragging(index, event)} 
      on:mouseup={() => stopDragging(index)}>
      
        <input
          type="text"
          bind:value={note.header}
          placeholder="Sticky Note"
          class="header-input"
          on:input={() => changeNoteHeader(index, note.header)}
        />
      </div>
      <textarea bind:value={note.text}></textarea>
     
			<button on:click={() => {
          const colorPicker = document.getElementById(`color-picker-${index}`);
          colorPicker.click();
        }} class="color-button">Note's Color</button>
       <input
          type="color"
          bind:value={note.backgroundColor}
          class="color-picker"
          id={`color-picker-${index}`}
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
    background-color: #445D48;
    padding: 8px;
    cursor: move;
    display: flex;
    justify-content: space-between;
  }

  .header-input {
    border: none;
    background: #445D48;
    font-size: 16px;
    font-family: Arial, sans-serif;
  }

  .color-button {
    background-color: #8B9A46;
		position: absolute;
		bottom: 10px;
		left: 10px;
    color: white;
    border: none;
    padding: 6px 12px;
    font-size: 14px;
    cursor: pointer;
    border-radius: 4px;
  }

	.color-picker {
    display: none;
  }

  textarea {
    width: calc(100% - 16px);
    height: calc(100% - 56px); 
    border: none;
    background: none;
    resize: none;
    margin: 8px;
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
