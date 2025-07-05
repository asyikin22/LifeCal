<script>
  import TopBar from '../components/TopBar.svelte';
  import LifeGrid from '../components/LifeGrid.svelte';
  import AnimatedBackground from '../components/backgrounds/AnimatedBackground.svelte';
  import AnimatedBackground2 from '../components/backgrounds/AnimatedBackground2.svelte';
  import AnimatedBackground3 from '../components/backgrounds/AnimatedBackground3.svelte';
  import PlainBackground from '../components/backgrounds/PlainBackground.svelte';
  import SocialBar from '../components/SocialBar.svelte';
  import { backgroundIndex } from '../stores.js';
  import { onMount } from 'svelte';

  const backgrounds = [AnimatedBackground, AnimatedBackground2, AnimatedBackground3, PlainBackground];
  
  let birthYear = '';
  let age = null;
  let allEvents = Array(50).fill().map(() => []); // Add this line
  let allCountries = Array(50).fill().map(() => ({ country: '', flag: '' })); // Add this line


  function updateAge(year) {
    const currentYear = new Date().getFullYear();
    age = year ? currentYear - year : null;
  }

  // Add the save and clear handlers from App.svelte
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
      localStorage.setItem('countries', JSON.stringify(allCountries));
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
    localStorage.removeItem('countries');
    birthYear = '';
    age = null;
    allEvents = Array(50).fill().map(() => []);
    allCountries = Array(50).fill().map(() => ({ country: '', flag: '' }));

    alert('Data cleared!');
  }

  // Optional: Load data on start
  if (typeof localStorage !== 'undefined' && localStorage.getItem('birthYear')) {
  birthYear = localStorage.getItem('birthYear');
  age = Number(localStorage.getItem('age'));
  allEvents = JSON.parse(localStorage.getItem('events')) || Array(50).fill().map(() => []);
  allCountries = JSON.parse(localStorage.getItem('countries')) || Array(50).fill().map(() => ({ country: '', flag: '' }));
}
</script>

<svelte:component this={backgrounds[$backgroundIndex]} />

<!-- TOP BAR at the top -->
<TopBar 
  bind:birthYear 
  bind:age 
  {updateAge}
  {allCountries}
  on:saveData={saveDataHandler}
  on:clearData={clearDataHandler}
/>

<!-- PROGRESS BAR in the middle -->
<div>
  {#if age !== null}
    <div class="progress-bar-container">
      <div 
        class="progress-bar" 
        style="width: {Math.min((age / 50) * 100, 100)}%">
      </div>
      <div class="progress-text">
        You've completed {Math.min(Math.round((age / 50) * 100), 100)}% of your first 50 years on earth! {Math.max(50 - age, 0)} years left to reach 50
      </div>
    </div>

    <div class="progress-label">50 years progress bar</div>
  {/if}
</div>

<SocialBar />

<LifeGrid 
  {birthYear} 
  {allEvents} 
  {allCountries}
  on:updateEvents={e => allEvents[e.detail.index] = e.detail.events}
  on:updateCountries={e => allCountries[e.detail.index] = e.detail.countries}
/>

<style>
  .progress-bar-container {
    width: 70%;
    background-color: red;
    border-radius: 8px;
    margin: 15px auto;
    overflow: hidden;
    height: 25px;
    position: relative;
    margin-bottom: 0;
  }

  .progress-bar {
    background-color: #0a820e;
    height: 150%;
    transition: width 0.3s ease;
  }

  .progress-text {
    position: absolute;
    width: 100%;
    text-align: center;
    top: 0;
    font-size: 15px;
    line-height: 25px;
    color: peachpuff;
    font-weight: bold;
  }

  .progress-label {
    margin-top: 2px;
    text-align: center;
    font-size: 15px;
    color: #eff62e;
    font-weight: bold;
  }

</style>