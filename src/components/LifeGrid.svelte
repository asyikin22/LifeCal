<script>
  import { createEventDispatcher } from 'svelte';
  import GridItem from './GridItem.svelte';
  
  export let birthYear;
  export let allEvents;
  export let allCountries;

  const dispatch = createEventDispatcher();

  function handleUpdateEvents(event) {
    dispatch('updateEvents', event.detail);
  }

  function handleUpdateCountries(event) {
    dispatch('updateCountries', event.detail);
  }
</script>

<div class="grid-container">
  {#if allEvents && allEvents.length > 0}
    {#each Array(50) as _, i}
      <GridItem
        {birthYear}
        age={i + 1}
        year={birthYear ? +birthYear + i + 1 : ''}
        bind:events={allEvents[i]}
        bind:selectedCountry={allCountries[i].country}
        bind:flag={allCountries[i].flag}
        on:update={(e) => {
          // Update events
          allEvents[i] = e.detail.events;
          handleUpdateEvents({ detail: { index: i, events: e.detail.events } });
          
          // Update countries
          allCountries[i] = { country: e.detail.country, flag: e.detail.flag };
          handleUpdateCountries({ detail: { index: i, countries: { country: e.detail.country, flag: e.detail.flag } } });
        }}
      />
    {/each}
  {/if}
</div>

<style>
  .grid-container {
    display: grid;
    grid-template-columns: repeat(5, 15%);
    gap: 8px;
    margin: 20px auto;
    justify-content: center;
  }
</style>