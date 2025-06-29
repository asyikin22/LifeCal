<script>
  import GridItem from './GridItem.svelte';
  export let birthYear;

  const totalBoxes = 50;
</script>

<div class="grid-container">
  {#each Array(totalBoxes) as _, i}
    <GridItem
      age={i + 1}
      year={birthYear ? birthYear + i + 1 : ''}
    />
  {/each}
</div>

<style>
  .grid-container {
  display: grid;
  grid-template-columns: repeat(5, minmax(20px, 1fr));
  gap: 6px;
  max-width: 70%;
  margin: 5px auto;
  justify-content: center;
  box-sizing: border-box;
}


</style>
