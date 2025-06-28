<script>
  import GridItem from './GridItem.svelte';
  export let birthYear;

  const totalBoxes = 70;
</script>

<div class="grid-container">
  {#each Array(totalBoxes) as _, i}
    <GridItem age={i} year={birthYear + i} />
  {/each}
</div>

<style>
  .grid-container {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    gap: 8px;
    max-width: 1000px;
    margin: 20px auto;
  }
</style>
