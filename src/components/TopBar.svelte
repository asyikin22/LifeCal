<script>
  import { createEventDispatcher, onDestroy } from 'svelte';

  export let birthYear;
  export let age;
  export let updateAge;

  const dispatch = createEventDispatcher();

  let now = new Date();

  const interval = setInterval(() => {
    now = new Date();
  }, 1000);

  $: day = now.toLocaleDateString(undefined, { weekday: 'long' });
  $: date = now.toLocaleDateString();
  $: time = now.toLocaleTimeString();

  $: timeZone = Intl.DateTimeFormat().resolvedOptions().timeZone;
  $: city = timeZone.includes('/') ? timeZone.split('/')[1].replace('_', ' ') : timeZone;

  $: experiencedPercentage = age !== null ? Math.min((age / 50) * 100, 100).toFixed(1) : null;
  $: yearsLeft = age !== null ? Math.max(50 - age, 0) : null;

  onDestroy(() => clearInterval(interval));

  function getGeneration(year) {
  const numericYear = Number(year);
  
  if (isNaN(numericYear)) return '';

  if (numericYear >= 1946 && numericYear <= 1964) {
    return 'Baby Boomer';
  } else if (numericYear >= 1965 && numericYear <= 1980) {
    return 'Gen X';
  } else if (numericYear >= 1981 && numericYear <= 1996) {
    return 'Millennial';
  } else if (numericYear >= 1997 && numericYear <= 2012) {
    return 'Gen Z';
  } else if (numericYear >= 2013 && numericYear <= 2025) {
    return 'Gen Alpha';
  } else if (numericYear > 2025) {
    return 'Future Generation';
  } else {
    return '';
  }
}

</script>

<div class="top-bar">
  <div class="box large">
    <h3 style="margin: 2px 0; font-size: 30px;">My Life Calendar</h3>
    <div style="font-size: 20px;">
      {#if age !== null}
        You are {age} years young! 😊
        <div style="font-size: 25px; margin-top: 10px;" class="rainbow-text">
          {getGeneration(birthYear)}
        </div>
      {/if}
    </div>
  </div>

  <div class="box small">
    <label for="birthYearInput">Birth Year:</label>
    <input
        id="birthYearInput"
        type="text"
        bind:value={birthYear}
        placeholder="e.g. 1990"
        on:input={() => {
            birthYear = birthYear.replace(/\D/g, '').slice(0, 4);

            const numericYear = Number(birthYear);
            const currentYear = new Date().getFullYear();

            if (birthYear.length === 4) {
                if (numericYear >= 1900 && numericYear <= currentYear) {
                updateAge(birthYear);
                } else {
                alert(`Please enter a valid year between 1900 and ${currentYear}`);
                birthYear = '';
                }
            }
            }}
        on:keydown={(e) => e.key === 'Enter' && updateAge(birthYear)}
    />

    <div class="button-row">
      <button on:click={() => dispatch('saveData')}>Save</button>
      <button on:click={() => dispatch('clearData')}>Clear</button>
    </div>
  </div>

  <div class="box small">
    <div class="timezone">{city}</div>
    <div>{day}, {date}</div>
    <div>{time}</div>
  </div>
</div>

<style>
  .top-bar {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    gap: 10px;
    margin: 20px auto;
  }

  .box {
    background-color: #f9f9f9;
    border: 1px solid #ccc;
    border-radius: 8px;
    padding: 15px;
    text-align: center;
    box-shadow: 2px 2px 6px rgba(0, 0, 0, 0.1);
    box-sizing: border-box;
    height: 120px;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }

  .box.large {
    width: 350px;
  }

  .box.small {
    width: 170px;
  }

  input, button {
    width: 100%;
    margin-top: 6px;
    padding: 6px;
    box-sizing: border-box;
    text-align: center;
  }

  .timezone {
    font-weight: bold;
    margin-bottom: 10px;
    font-size: 20px;
    text-transform: capitalize;
  }

  .button-row {
    display: flex;
    gap: 6px;
    margin-top: 5px;
    justify-content: center;
    flex-wrap: wrap;
  }

  .button-row button {
    flex: 1;
    padding: 6px 6px;
    font-size: 10px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    background-color: rgb(175, 169, 169);
    transition: background-color 0.2s ease;
  }

  .button-row button:hover {
    background-color: rgb(137, 134, 134);
  }

  .rainbow-text {
    background: linear-gradient(90deg, #8e2de2, #4a00e0, #007cf0, #00dfd8, #8e2de2);
    background-size: 300% 300%;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    animation: galaxyShift 8s ease infinite;
  }

  @keyframes galaxyShift {
    0% { background-position: 0% 50%; }
    50% { background-position: 100% 50%; }
    100% { background-position: 0% 50%; }
  }

</style>
