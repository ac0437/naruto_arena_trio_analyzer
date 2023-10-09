<!-- @format -->
<script>
  import { CharacterStore } from './character_store';
  import { SelectedCharacterStore } from './selected_character_store';
  import Card from './components/Card.svelte';
  import CharacterFilter from './components/CharacterFilter.svelte';
  // import WarFilter from './components/WarFilter.svelte';
  import WarModal from './components/WarModal.svelte';
</script>

<main>
  <CharacterFilter />
  <!-- <WarFilter /> -->
  <p>Currently {$SelectedCharacterStore.length}/3 characters selected</p>
  <div class="card-container">
    {#each $CharacterStore as character (character.id)}
      <Card {character} />
    {/each}
  </div>
  {#if $SelectedCharacterStore.length > 2}
    <WarModal />
  {/if}
</main>

<style>
  main {
    position: relative;
  }
  .card-container {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 0.5em;
  }
  p {
    font-weight: 500;
  }
  @media (max-width: 640px) {
    .card-container {
      grid-template-columns: repeat(1, 1fr);
    }
  }
</style>
