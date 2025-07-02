<script>
  import TopBar from './components/TopBar.svelte';

  let birthYear = '';
  let age = null;

  function updateAge(year) {
    const currentYear = new Date().getFullYear();
    const numericYear = Number(year);

    if (isNaN(numericYear) || numericYear < 1900 || numericYear > currentYear) {
      alert(`Please enter a valid year between 19xx and ${currentYear}`);
      return;
    }
    age = currentYear - numericYear;
  }

  // function saveDataHandler() {
  //   if (birthYear) {
  //     localStorage.setItem('birthYear', birthYear);
  //     alert('Data saved!');
  //   } else {
  //     alert('Enter a valid birth year before saving!');
  //   }
  // }

  // function clearDataHandler() {
  //   localStorage.removeItem('birthYear');
  //   birthYear = '';
  //   age = null;
  //   alert('Data cleared!');
  // }

  function saveDataHandler() {
  console.log('Save clicked'); // Debugging line
  // ...existing save logic...
}

function clearDataHandler() {
  console.log('Clear clicked'); // Debugging line
  // ...existing clear logic...
}
</script>

<TopBar 
  bind:birthYear={birthYear}
  {age} 
  {updateAge}
  on:saveData={saveDataHandler}
  on:clearData={clearDataHandler}
/>






