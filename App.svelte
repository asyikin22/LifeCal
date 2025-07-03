<script>
  import TopBar from './components/TopBar.svelte';
  import LifeGrid from './components/LifeGrid.svelte';

  let birthYear = '';
  let age = null;
  let allEvents = Array(50).fill().map(() => []); // This was missing!


  function updateAge(year) {
    const currentYear = new Date().getFullYear();
    const numericYear = Number(year);

    if (isNaN(numericYear) || numericYear < 1900 || numericYear > currentYear) {
      alert(`Please enter a valid year between 19xx and ${currentYear}`);
      return;
    }
    age = currentYear - numericYear;
  }

  function saveDataHandler() {
    console.log('Save button clicked!');
    console.log('birthYear:', birthYear);
    console.log('age:', age);
    console.log('allEvents:', allEvents);
    console.log('saveDataHandler called');

    if (birthYear) {
      localStorage.setItem('birthYear', birthYear);
      localStorage.setItem('age', age);
      localStorage.setItem('events', JSON.stringify(allEvents));
      alert('Data saved!');
    } else {
      alert('Enter a valid birth year before saving!');
    }
  }

  function clearDataHandler() {

    console.log('clearDataHandler called');
    
    localStorage.removeItem('birthYear');
    localStorage.removeItem('age');
    localStorage.removeItem('events');
    birthYear = '';
    age = null;
    allEvents = Array(50).fill().map(() => []);
    alert('Data cleared!');
  }

  // Optional: Load data on start
  if (localStorage.getItem('birthYear')) {
    birthYear = localStorage.getItem('birthYear');
    age = Number(localStorage.getItem('age'));
    allEvents = JSON.parse(localStorage.getItem('events')) || Array(50).fill().map(() => []);
  }

</script>

<TopBar 
  bind:birthYear
  {age} 
  {updateAge}
  on:saveData={saveDataHandler}
  on:clearData={clearDataHandler}
/>

<LifeGrid {birthYear} {allEvents} on:updateEvents={e => allEvents[e.detail.index] = e.detail.events} />







