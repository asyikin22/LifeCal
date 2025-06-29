<script>
  export let birthYear;
  export let age;
  export let updateAge;

  import { onDestroy } from 'svelte';

  let now = new Date();

  // Update time every second
  const interval = setInterval(() => {
    now = new Date();
  }, 1000);

  // Reactive values
  $: day = now.toLocaleDateString(undefined, { weekday: 'long' });
  $: date = now.toLocaleDateString();
  $: time = now.toLocaleTimeString();

  $: timeZone = Intl.DateTimeFormat().resolvedOptions().timeZone;
  $: city = timeZone.includes('/') ? timeZone.split('/')[1].replace('_', ' ') : timeZone;

  $: experiencedPercentage = age !== null ? Math.min((age / 50) * 100, 100).toFixed(1) : null;
  $: yearsLeft = age !== null ? Math.max(50 - age, 0) : null;


  onDestroy(() => clearInterval(interval));
</script>

<div class="top-bar">
  <div class="box large">
    <h3 style="margin: 2px 0; font-size: 30px;">My Life Calendar</h3>
    <div style="font-size: 15px;">
      {#if age !== null}
        You are {age} years young! 😊
      {/if}
      {#if age !== null}
        <div style="font-size: 15px; margin-top: 4px; 4px;">
          You've experienced <span style="font-size: 18px; font-weight: bold;">{Math.round(experiencedPercentage)}%</span> of your first 50 years.<br>
          {#if yearsLeft > 0}
            <span style="font-size: 18px; font-weight: bold;">{yearsLeft}</span> years left before you hit 50!
          {:else}
            You have surpassed 50. A living legend! 🎉
          {/if}
        </div>
      {/if}
    </div>
  </div>

  <div class="box small">
    <label for="birthYearInput">Birth Year:</label>
    <input id="birthYearInput" type="number" bind:value={birthYear} placeholder="e.g. 1990" on:keydown={(e) => e.key === 'Enter' && updateAge(birthYear)} />
    <button on:click={() => updateAge(birthYear)}>Generate Calendar</button>
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

/* First box larger */
.box.large {
  width: 350px;
}

/* Other boxes smaller */
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

</style>
