<!-- @format -->
<script>
  import { CharacterStore } from '../character_store';

  let characterName = '';
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
</script>

<input
  bind:value={characterName}
  on:input={() => filterCharacter()}
  placeholder="search a character name"
/>
