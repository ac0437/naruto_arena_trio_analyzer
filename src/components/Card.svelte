<!-- @format -->
<script>
  import { CharacterStore } from '../character_store';
  import { SelectedCharacterStore } from '../selected_character_store';
  export let character;
  const originalStore = $CharacterStore.map((character) => character);
  $: selected = [];

  function selectedCharacter(name) {
    let index = originalStore.map((character) => character.name).indexOf(name);
    let selectedCharacter = originalStore[index];
    SelectedCharacterStore.update((characters) => {
      let checkIfCharacterIsSelected = characters
        .map((character) => character.name)
        .indexOf(name);

      if (checkIfCharacterIsSelected === -1) {
        characters.push(selectedCharacter);
        selected = characters.map((character) => character.name);
        return characters;
      } else if (checkIfCharacterIsSelected > -1) {
        let newSelectedCharacters = characters.filter(
          (selectedCharacter) => selectedCharacter.name != name
        );
        selected = newSelectedCharacters.map((character) => character.name);
        return newSelectedCharacters;
      }
    });
  }
</script>

<button
  class={$SelectedCharacterStore
    .map((character) => character.name)
    .indexOf(character.name) !== -1
    ? 'container selected'
    : 'container'}
  on:click={() => selectedCharacter(character.name)}
>
  <h2>{character.name}</h2>
  <slot />
</button>

<style>
  .container {
    border-radius: 12px;
    border: 1px solid #ccc;
    padding: 2em;
    background-color: transparent;
    cursor: pointer;
  }
  .container:hover {
    background-color: #ccc;
    border: 1px solid #000;
  }
  h2 {
    margin: 0;
    padding-bottom: 1em;
  }
  .selected {
    background-color: darkgreen;
    color: white;
    border: 1px solid #000;
  }
  .container.selected:hover {
    background-color: burlywood;
  }
</style>
