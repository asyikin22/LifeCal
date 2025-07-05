<script>
  import { createEventDispatcher, onDestroy } from 'svelte';
  import { Save, RotateCcw } from 'lucide-svelte'; // Import Save and Trash icons

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
    return { label: 'Baby Boomer', url: 'https://en.wikipedia.org/wiki/Baby_boomers' };
  } else if (numericYear >= 1965 && numericYear <= 1980) {
    return { label: 'Gen X', url: 'https://en.wikipedia.org/wiki/Generation_X' };
  } else if (numericYear >= 1981 && numericYear <= 1996) {
    return { label: 'Millennial', url: 'https://en.wikipedia.org/wiki/Millennials' };
  } else if (numericYear >= 1997 && numericYear <= 2012) {
    return { label: 'Gen Z', url: 'https://en.wikipedia.org/wiki/Generation_Z' };
  } else if (numericYear >= 2013 && numericYear <= 2025) {
    return { label: 'Gen Alpha', url: 'https://en.wikipedia.org/wiki/Generation_Alpha' };
  } else if (numericYear > 2025) {
    return { label: 'Future Generation', url: '#' };
  } else {
    return null;
  }
}

</script>

<div class="top-bar">
  <div class="box large">
    <h3 style="margin: 2px 0; font-size: 30px; margin-bottom: 7px; color:brown;">0.5 Century Log</h3>
    <div style="font-size: 18px;">
      {#if age !== null}
        You are {age} years young!
        {#if getGeneration(birthYear)}
          <div style="font-size: 25px; margin-top: 10px; font-weight: bold;">
            <a
              href="{getGeneration(birthYear).url}"
              target="_blank"
              rel="noopener noreferrer"
              class="rainbow-text"
              style="text-decoration: none;"
            >
              {getGeneration(birthYear).label}
            </a>
          </div>
        {/if}
      {/if}
    </div>
  </div>

  <div class="box small">
    <label for="birthYearInput" style="font-weight: bold;">Birth Year:</label>
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
      <button on:click={() => {
        console.log('Save button clicked in TopBar');
        dispatch('saveData');
      }} title="Save">
        <Save size="15" />
      </button>
      <button on:click={() => {
        console.log('Clear button clicked in TopBar');
        dispatch('clearData');
      }} title="Clear">  
        <RotateCcw size="15" />
      </button>
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
  padding: 10px;
  border-radius: 12px;
}

.box {
  background: rgba(255, 255, 255, 0.6);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 10px;
  padding: 10px;
  text-align: center;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  justify-content: center;
  backdrop-filter: blur(5px);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  color: #222; /* Darker text for contrast */
  font-weight: 600;
}

.box.large {
  width: 350px;
}

.box.small {
  width: 180px;
}

input {
  width: 100%;
  background-color: rgba(255, 255, 255, 0.3);
  margin-top: 6px;
  padding: 6px;
  box-sizing: border-box;
  text-align: center;
  border-radius: 4px;
  border: 1px solid #ccc;
}

input:focus {
  outline: none;
  border-color: #aaa;
}

.timezone {
  font-weight: bold;
  margin-bottom: 10px;
  font-size: 20px;
  text-transform: capitalize;
  color: brown;
}

.button-row {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-top: 6px;
}

.button-row button {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 6px;
  background: rgba(128, 172, 226, 0.3);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 6px;
  cursor: pointer;
  transition: transform 0.1s ease, background-color 0.2s ease;
}

.button-row button:hover {
  background-color: rgba(137, 134, 134, 0.5);
  transform: scale(1.1);
}

.button-row button:active {
  transform: scale(0.99);
  background-color: rgba(100, 100, 100, 0.7); /* Optional darker shade on click */
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
