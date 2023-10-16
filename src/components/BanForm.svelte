<!-- @format -->
<script>
  function onSubmit() {
    const name = document.getElementById('name').value;
    const characterList = document
      .getElementById('character-list')
      .value.split(',');
    const getBanLists = JSON.parse(localStorage.getItem('banlists'));
    if (getBanLists[name] !== undefined) {
      return;
    }
    getBanLists[name] = characterList;
    localStorage.setItem('banlists', JSON.stringify(getBanLists));
  }
</script>

<div class="backdrop">
  <form class="container" on:submit={() => onSubmit()}>
    <label for="name">Name</label>
    <input type="text" id="name" />
    <textarea
      id="character-list"
      name="character"
      cols="30"
      rows="10"
      placeholder="Create a comma seperated list with the full name of the character you want banned.&#10;&#10;Example: Uzumaki Naruto, Uchiha Sasuke S, Haruno Sasukra TL."
    />
    <button type="submit">Save</button>
  </form>
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
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
</style>
