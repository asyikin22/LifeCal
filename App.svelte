<script>
  import TopBar from './components/TopBar.svelte';
  import LifeGrid from './components/LifeGrid.svelte';

  let birthYear = '';
  let age = '';
  
  function updateAge() {
    const currentYear = new Date().getUTCFullYear();
    if (birthYear && !isNaN(birthYear)) {
      age = currentYear - birthYear;
    } else {
      age = '';
    }
  }
</script>

<TopBar bind:birthYear {age} on:updateAge={updateAge} />
<LifeGrid {birthYear} />
