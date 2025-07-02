<script>
  import GridItem from './GridItem.svelte';
  export let birthYear;

  // Create an array of 50 empty arrays for each year's events
  let allEvents = Array(50).fill().map(() => []);
</script>

<div class="grid-container">
  {#each Array(50) as _, i}
    <GridItem
      {birthYear}
      age={i + 1}
      year={birthYear ? +birthYear + i + 1 : ''}
      bind:events={allEvents[i]}
    />
  {/each}
</div>

<style>
  .grid-container {
    display: grid;
    grid-template-columns: repeat(5, 13%);
    gap: 8px;
    margin: 20px auto;
    justify-content: center;
  }
</style>
