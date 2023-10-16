<!-- @format -->
<script>
  import { onMount } from 'svelte';
  import { CharacterStore } from '../character_store';
  const originalStore = structuredClone($CharacterStore);

  const banListKeys = Object.keys(JSON.parse(localStorage.getItem('banlists')));

  function onSelection(e) {
    const key = e.target.value;
    if (key.toLowerCase() === 'default') {
      CharacterStore.update((characters) => {
        return originalStore;
      });
    } else {
      let banList = JSON.parse(localStorage.getItem('banlists'))[key];
      CharacterStore.update((characters) => {
        $CharacterStore.forEach((character) => {
          const findIfCharacterIsBanned = banList.find(
            (bannedCharacter) =>
              character.name.toLowerCase() ===
              bannedCharacter.toLowerCase().trim()
          );
          if (findIfCharacterIsBanned) {
            character.banned = 'Yes';
          } else {
            character.banned = 'No';
          }
        });

        return $CharacterStore;
      });
    }
  }
</script>

<select on:change={(e) => onSelection(e)}>
  <option>Default</option>
  {#each banListKeys as key, i}
    <option value={key}>
      {key}
    </option>
  {/each}
</select>
