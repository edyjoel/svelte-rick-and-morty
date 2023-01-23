<script>
  import Character from "./lib/Character.svelte";

  let characters = [];
  let page = 1;
  let allPages = 1;
  let isLoading = true;

  async function loadCharacters() {
    isLoading = true;
    const response = await fetch(
      "https://rickandmortyapi.com/api/character?page=" + page
    );
    const data = await response.json();
    allPages = data.info.pages;
    characters = data.results;
    isLoading = false;
  }

  loadCharacters();

  function nextPage() {
    page++;
    loadCharacters();
  }

  function previousPage() {
    page--;
    loadCharacters();
  }
</script>

<div class="container">
  <h1 class="title">Rick and Morty Svelte</h1>
  <div class="btns">
    <button class="btn" on:click={previousPage} disabled={page === 1}
      >Previous</button
    >
    <span class="page">{page}/{allPages}</span>
    <button class="btn" on:click={nextPage} disabled={page === allPages}
      >Next</button
    >
  </div>
  {#if isLoading}
    <div class="spinner" />
  {:else}
    <div class="grid">
      {#each characters as character}
        <Character {character} />
      {/each}
    </div>
  {/if}
</div>
