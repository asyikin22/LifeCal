<script>
  import TopBar from '../components/TopBar.svelte';
  import LifeGrid from '../components/LifeGrid.svelte';

  let birthYear = null;
  let age = null;

  function updateAge(year) {
    const now = new Date().getUTCFullYear();
    birthYear = year;
    age = now - year;
  }
</script>

<TopBar {birthYear} {age} {updateAge} />
{#if birthYear}
  <LifeGrid {birthYear} />
{/if}
