<!-- @format -->
<script>
  import BanListSelect from './BanListSelect.svelte';
  import BanForm from './BanForm.svelte';
  import { CharacterStore } from '../character_store';
  import { SelectedCharacterStore } from '../selected_character_store';
  let characterName = '';
  let createBanList = false;
  const originalStore = $CharacterStore.map((character) => character);

  function filterCharacter() {
    CharacterStore.update((characters) => {
      let original = originalStore;
      let filtered = original.filter((character) =>
        character.name.toLowerCase().includes(characterName.toLowerCase())
      );
      return filtered;
    });
  }

  function showBanListForm() {
    createBanList = true;
  }

  function closeBanListForm() {
    createBanList = false;
  }
</script>

<div class="container">
  <input
    bind:value={characterName}
    on:input={() => filterCharacter()}
    placeholder="Search a character name"
  />
  <p>
    {#if $SelectedCharacterStore.length == 0}
      Currently selected 0/3
    {:else if $SelectedCharacterStore.length > 0}Currently selected <span
        >{$SelectedCharacterStore
          .map((character) => character.name)
          .join(', ')}</span
      >
      {$SelectedCharacterStore.length}/3
    {/if}
  </p>
  <BanListSelect />
  <button on:click={() => showBanListForm()}>Create a ban list (beta)</button>
  {#if createBanList}
    <button class="close-form" on:click={() => closeBanListForm()}
      ><span class="material-symbols-outlined">cancel</span></button
    >
    <BanForm />
  {/if}
</div>

<style>
  .container {
    width: 100%;
    position: sticky;
    top: 0;
    background-color: #fff;
    display: flex;
    justify-content: center;
    gap: 1em;
    padding: 0.5em;
    z-index: 999;
  }
  input {
    margin: 0;
  }
  p {
    display: inline-block;
  }
  span {
    font-weight: 500;
  }
  .close-form {
    position: absolute;
    top: 0;
    right: 0;
    z-index: 999999;
    color: white;
    background: transparent;
    border: none;
  }
</style>
