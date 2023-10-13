<!-- @format -->
<script>
  import { CharacterStore } from '../character_store';
  import { SelectedCharacterStore } from '../selected_character_store';
  export let character;
  export let illegalTeam = false;
  export let bannedCharacters = '';
  const originalStore = $CharacterStore.map((character) => character);
  $: selected = [];

  console.log(bannedCharacters);
  console.log('illegal team', illegalTeam);
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

  function removeCharacter(name) {
    console.log('removing', name);
    SelectedCharacterStore.update((characters) => {
      let newSelectedCharacters = characters.filter(
        (selectedCharacter) => selectedCharacter.name != name
      );
      selected = newSelectedCharacters.map((character) => character.name);
      return newSelectedCharacters;
    });
  }
</script>

<div class="container">
  <button
    class={$SelectedCharacterStore
      .map((character) => character.name)
      .indexOf(character.name) !== -1 &&
    bannedCharacters.includes(character.name) !== true
      ? 'btn selected'
      : character.banned === 'Yes' &&
        bannedCharacters.includes(character.name) === true
      ? 'btn banned'
      : illegalTeam === true && bannedCharacters.includes(character.name)
      ? 'btn illegal'
      : 'btn'}
    on:click|stopPropagation={() => selectedCharacter(character.name)}
  >
    {#if $SelectedCharacterStore.length > 0 && $SelectedCharacterStore
        .map((character) => character.name)
        .indexOf(character.name) !== -1}
      <button
        class="close-btn"
        on:click|stopPropagation={() => removeCharacter(character.name)}
        disabled={$SelectedCharacterStore.length > 0 &&
        $SelectedCharacterStore
          .map((character) => character.name)
          .indexOf(character.name) !== -1
          ? false
          : true}
        ><span class="material-symbols-outlined"> cancel </span></button
      >
    {/if}
    <img
      src={`images/${character.name.replaceAll(/\s/g, '_')}.png`}
      alt={character.name}
    />
    <h2>
      {character.name}{#if character.banned === 'Yes'} - Banned{/if}
    </h2>
    <slot />
  </button>
</div>

<style>
  .container {
    border-radius: 12px;
    position: relative;
  }
  .btn {
    background-color: transparent;
    cursor: pointer;
    width: 100%;
    height: 100%;
    border-radius: 12px;
  }
  .btn:hover {
    background-color: #ccc;
    border: 1px solid #000;
  }
  .close-btn {
    position: absolute;
    top: 10px;
    right: 10px;
    border-radius: 1em;
    background-color: transparent;
    cursor: pointer;
    border: none;
    z-index: 99;
    height: 48px;
    width: 48px;
    border-radius: 100%;
    color: white;
  }
  img {
    height: auto;
  }
  h2 {
    margin: 0;
    padding-bottom: 1em;
  }
  .selected {
    border: 1px solid darkgreen;
    background-color: green;
    color: white;
  }
  .banned {
    border: 1px solid rebeccapurple;
    background-color: red;
    color: white;
  }
  .illegal {
    background-color: orange;
    color: white;
  }
</style>
