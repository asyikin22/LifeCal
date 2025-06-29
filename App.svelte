<script>
  import TopBar from './components/TopBar.svelte';
  import LifeGrid from './components/LifeGrid.svelte';

  let birthYear = '';
  let age = '';
  let gridKey = 0; // used to force LifeGrid reset

  function updateAge() {
    const currentYear = new Date().getFullYear();
    age = birthYear && !isNaN(birthYear) ? currentYear - birthYear : '';
  }

  function saveData() {
    localStorage.setItem('lifeGridData', JSON.stringify({ birthYear, age }));
    alert('Data saved successfully!');
  }

  function clearData() {
    localStorage.removeItem('lifeGridData');
    birthYear = '';
    age = '';
    gridKey += 1; // forces LifeGrid to reset
    alert('Data cleared!');
  }
</script>

<TopBar
  bind:birthYear
  {age}
  {updateAge}
  {saveData}
  {clearData}
/>

{#key gridKey}
  <LifeGrid {birthYear} />
{/key}
