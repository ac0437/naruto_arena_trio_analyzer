<!-- @format -->
<script>
  import { onMount } from 'svelte';
  import { SelectedCharacterStore } from '../selected_character_store';
  import Card from './Card.svelte';
  let analysisTeamText = '';
  let banAnalysisText = '';
  let stunAnalysisText = '';
  let chakraDrainAnalysisText = '';
  let aoeAnalysisText = '';
  let healerAnalysisText = '';
  let counterOrReflectAnalysisText = '';
  let afflictionAnalysisText = '';
  let analysisArray = [];

  let illegalTrio = true;
  let bannedCharacters = '';
  let checkForBannedCharacters = $SelectedCharacterStore.filter(
    (character) => character.banned === 'Yes'
  );
  let checkForStunners = $SelectedCharacterStore.filter(
    (character) => character.stun === 'Yes'
  );
  let checkForChakraDrain = $SelectedCharacterStore.filter(
    (character) => character.chakra_drain === 'Yes'
  );
  let checkForAoe = $SelectedCharacterStore.filter(
    (character) => character.aoe === 'Yes'
  );
  let checkForHealers = $SelectedCharacterStore.filter(
    (character) => character.heal === 'Yes'
  );
  let checkForCountersOrReflects = $SelectedCharacterStore.filter(
    (character) => character.counter_reflect === 'Yes'
  );
  let checkForAffliction = $SelectedCharacterStore.filter(
    (character) => character.skill_effects.indexOf('Affliction') !== -1
  );

  onMount(() => {
    if (checkForStunners.length > 2) {
      const stunners = checkForStunners.map((character) => character.name);
      const illegalCharacters = stunners.join(', ');
      bannedCharacters += illegalCharacters;
      stunAnalysisText = `Full stun team`;
      analysisArray = [...analysisArray, stunAnalysisText];
    }
    if (checkForChakraDrain.length > 1) {
      const checkForChakraDrainers = checkForChakraDrain.map(
        (character) => character.name
      );
      const chakraDrainers = checkForChakraDrainers.join(', ');
      const illegalCharacters = chakraDrainers;
      bannedCharacters += illegalCharacters;
      chakraDrainAnalysisText = `Chakra drainers: ${chakraDrainers}`;
      analysisArray = [...analysisArray, chakraDrainAnalysisText];
    }
    if (checkForAoe.length > 2) {
      const checkAoE = checkForAoe.map((character) => character.name);
      const illegalCharacters = checkAoE.join(', ');
      bannedCharacters += illegalCharacters;
      aoeAnalysisText = `Full AoE team`;
      analysisArray = [...analysisArray, aoeAnalysisText];
    }
    if (checkForHealers.length > 2) {
      const healers = checkForHealers.map((character) => character.name);
      const illegalCharacters = healers.join(', ');
      bannedCharacters += illegalCharacters;
      healerAnalysisText = `Full healer team`;
      analysisArray = [...analysisArray, healerAnalysisText];
    }
    if (checkForCountersOrReflects.length > 2) {
      const counterOrReflect = checkForCountersOrReflects.map(
        (character) => character.name
      );
      const illegalCharacters = counterOrReflect.join(', ');
      bannedCharacters += illegalCharacters;
      counterOrReflectAnalysisText = `Full counter/reflect team`;
      analysisArray = [...analysisArray, counterOrReflectAnalysisText];
    }
    if (checkForAffliction.length > 2) {
      const afflictionCharacters = checkForAffliction.map(
        (character) => character.name
      );
      const illegalCharacters = afflictionCharacters.join(', ');
      bannedCharacters += illegalCharacters;
      afflictionAnalysisText = `Full affliction team`;
      analysisArray = [...analysisArray, afflictionAnalysisText];
    }
    if (checkForBannedCharacters.length >= 1) {
      const checkForCharacters = checkForBannedCharacters.map(
        (character) => character.name
      );
      const illegalCharacters = checkForCharacters.join(', ');
      bannedCharacters += illegalCharacters;
      banAnalysisText = `Banned character(s): ${illegalCharacters} are banned`;
      analysisArray = [...analysisArray, banAnalysisText];
    }
    if (
      checkForBannedCharacters.length < 1 &&
      checkForStunners.length < 3 &&
      checkForChakraDrain.length < 2 &&
      checkForAoe.length < 3 &&
      checkForHealers.length < 3 &&
      checkForAffliction.length < 3 &&
      checkForCountersOrReflects.length < 3
    ) {
      analysisTeamText = `This team is legal and ready to fight!`;
      illegalTrio = false;
    }
  });
</script>

<div class="backdrop">
  <div class="container">
    <h2
      class={analysisTeamText !== 'This team is legal and ready to fight!'
        ? 'red'
        : 'green'}
    >
      {#if analysisTeamText !== ''}
        {analysisTeamText}
      {:else}
        Team is illegal because:
      {/if}
    </h2>
    <ol class="reason-list">
      {#each analysisArray as reason, i}
        <li class="reason-item">{reason}</li>
      {/each}
    </ol>
    <div class="card-container">
      {#each $SelectedCharacterStore as character (character.id)}
        <Card {character} illegalTeam={illegalTrio} {bannedCharacters}>
          <div>
            <ul>
              <h3>War Categories</h3>
              <li>Banned: {character.banned}</li>
              <li>Stunner: {character.stun}</li>
              <li>Chakra Drainer: {character.chakra_drain}</li>
              <li>AOE: {character.aoe}</li>
              <li>Healer: {character.heal}</li>
              <li>Counter: {character.counter_reflect}</li>
            </ul>
            <ul>
              <h3>Skill effects</h3>
              {#each character.skill_effects as effect, i}
                <li>{effect}</li>
              {/each}
            </ul>
          </div>
        </Card>
      {/each}
    </div>
  </div>
</div>

<style>
  .backdrop {
    position: fixed;
    left: 0;
    top: 0;
    background-color: rgba(0, 0, 0, 0.4);
    z-index: 1;
    width: 100%;
    height: 100%;
    overflow: auto;
    display: grid;
    place-items: center;
    z-index: 99999;
  }
  .container {
    background-color: #fff;
    border: 1px solid #ccc;
    border-radius: 12px;
    padding: 1em;
  }
  .card-container {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 0.5em;
  }
  h2 {
    margin: 0;
  }
  .red {
    color: red;
  }
  .green {
    color: green;
  }
  .reason-list {
    margin: 0;
    padding-bottom: 1em;
  }
  .reason-item {
    font-weight: 500;
  }
  @media (max-width: 640px) {
    .card-container {
      grid-template-columns: repeat(1, 1fr);
    }
  }
</style>
