<!-- @format -->
<script>
  import { CharacterStore } from '../character_store';
  let characterName = '';
  const originalStore = $CharacterStore.map((character) => character);
  const keys = {
    Banned: 'banned',
    Stunner: 'stun',
    'Chakra Drainer': 'chakra_drain',
    AOE: 'aoe',
    Healer: 'heal',
    'Counter/Reflect': 'counter_reflect',
    Reset: 'reset',
  };
  function filterCharacters(e) {
    const key = keys[e.target.textContent];
    console.log(e.target.textContent);
    CharacterStore.update((characters) => {
      if (key === 'reset') {
        return originalStore;
      }
      let filtered = characters.filter((character) => character[key] == 'Yes');
      return filtered;
    });
  }
</script>

<div class="chip-container">
  <button class="btn" on:click={(e) => filterCharacters(e)}>Banned</button>
  <button class="btn" on:click={(e) => filterCharacters(e)}>Stunner</button>
  <button class="btn" on:click={(e) => filterCharacters(e)}
    >Chakra Drainer</button
  >
  <button class="btn" on:click={(e) => filterCharacters(e)}>AOE</button>
  <button class="btn" on:click={(e) => filterCharacters(e)}>Healer</button>
  <button class="btn" on:click={(e) => filterCharacters(e)}
    >Counter/Reflect</button
  >
  <button class="btn" on:click={(e) => filterCharacters(e)}>Reset</button>
</div>

<style>
  .btn {
    border-radius: 2em;
    font-size: 12px;
    padding: 0.5em 1em;
    font-weight: 600;
  }
</style>
