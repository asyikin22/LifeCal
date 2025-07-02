<script>
  import { countryList } from '../countries.js';
  import Modal from './Modal.svelte';

  export let age;
  export let year;
  export let events = [];

  let selectedCountry = '';
  let flag = '';
  
  let showEventModal = false;
  let showCountryModal = false;
  
  let selectedType = '';
  let detail = '';
  
  const eventTypes = [
    { type: 'education', emoji: '🎓', color: '#FFD700' },
    { type: 'work', emoji: '💼', color: '#ADD8E6' },
    { type: 'travel', emoji: '✈️', color: '#90EE90' },
    { type: 'relationship', emoji: '💍', color: '#FFB6C1' },
    { type: 'loss', emoji: '🖤', color: '#D3D3D3' },
    { type: 'other', emoji: '⭐', color: '#FFA500' }
  ];

  function handleCountryChange(event) {
    const selectedOption = event.target.selectedOptions[0];
    const code = selectedOption.value;
    const name = selectedOption.getAttribute('data-name');

    if (code) {
      flag = `https://flagcdn.com/w20/${code.toLowerCase()}.png`;
      selectedCountry = name;
      showCountryModal = false;
    }
  }

  function allowCountryChange() {
    flag = '';
    selectedCountry = '';
  }

  function confirmAddEvent() {
    if (selectedType && detail) {
      const selected = eventTypes.find(e => e.type === selectedType);
      if (selected) {
        events = [...events, { emoji: selected.emoji, detail, color: selected.color }];
      }
      resetEventForm();
    }
  }

  function cancelEvent() {
    resetEventForm();
  }

  function resetEventForm() {
    showEventModal = false;
    selectedType = '';
    detail = '';
  }

  function deleteEvent(index) {
    events = events.filter((_, i) => i !== index);
  }
</script>

<div class="grid-item">
  <div class="age-year">
    <div class="age-text">Age: {age}</div>
    <div class="year-text">{year}</div>
  </div>

  {#each events as event, i}
    <div class="event-block" style="background-color: {event.color}">
      {event.emoji} {event.detail}
      <button type="button" on:click={() => deleteEvent(i)} class="delete-btn">✖</button>
    </div>
  {/each}

  <div class="spacer"></div>
  <button class="add-btn" on:click={() => showEventModal = true}>+</button>

  {#if flag}
    <button class="flag-btn" on:click={allowCountryChange} title={selectedCountry} aria-label="Change country">
      <img src={flag} alt={selectedCountry} class="flag" />
    </button>
  {:else}
    <button class="globe-btn" on:click={() => showCountryModal = true} aria-label="Select country">
      🌍
    </button>
  {/if}
</div>

<!-- Event Modal -->
<Modal show={showEventModal} title="Add Life Event" onClose={cancelEvent}>
  <select bind:value={selectedType} class="event-select">
    <option value="">Select event type</option>
    {#each eventTypes as e}
      <option value={e.type}>{e.emoji} {e.type}</option>
    {/each}
  </select>

  <input
    type="text"
    bind:value={detail}
    placeholder="Event details"
    class="detail-input"
    on:keydown={(e) => e.key === 'Enter' && confirmAddEvent()}
  />

  <div class="event-actions">
    <button on:click={confirmAddEvent}>Add</button>
    <button on:click={cancelEvent} class="cancel-btn">Cancel</button>
  </div>
</Modal>

<!-- Country Modal -->
<Modal show={showCountryModal} title="Select Country" onClose={() => showCountryModal = false}>
  <select on:change={handleCountryChange} class="country-select">
    <option value="">Select country...</option>
    {#each countryList as country}
      <option value={country.code} data-name={country.name}>{country.name}</option>
    {/each}
  </select>
</Modal>

<style>
  .grid-item {
    border: 1px solid #999;
    padding: 6px;
    padding-bottom: 24px;
    font-size: 12px;
    min-height: 100px;
    display: flex;
    flex-direction: column;
    align-items: center;
    position: relative;
    background: #fff;
  }

  .age-year {
    margin-bottom: 6px;
    text-align: center;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 4px;
    flex-wrap: wrap;
    width: 100%;
    position: relative;
  }

  .flag-btn {
    background: transparent;
    border: none;
    cursor: pointer;
    padding: 0;
    position: absolute;
    left: 4px;
    bottom: 4px;
    transition: filter 0.2s ease;
  }

  .flag-btn:hover {
    filter: brightness(1.2);
  }

  .flag {
    width: 20px;
    height: 14px;
  }

  .globe-btn {
    background: transparent;
    border: none;
    cursor: pointer;
    padding: 0;
    position: absolute;
    left: 4px;
    bottom: 4px;
    font-size: 16px;
  }

  .add-btn {
    font-size: 12px;
    cursor: pointer;
    position: absolute;
    bottom: 4px;
    right: 4px;
  }

  .spacer {
    height: 5px;
  }

  .event-block {
    margin-top: 4px;
    padding: 2px 4px;
    width: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 12px;
    position: relative;
    color: #333;
  }

  .delete-btn {
    cursor: pointer;
    margin-left: 6px;
    background: transparent;
    border: none;
    opacity: 0.3;
    transition: opacity 0.2s ease;
  }

  .delete-btn:hover {
    opacity: 1;
  }

  .event-select {
    width: 100%;
    margin-top: 4px;
  }

  .detail-input {
    margin-top: 4px;
    width: 100%;
    font-size: 12px;
    padding: 2px 4px;
    box-sizing: border-box;
  }

  .event-actions {
    display: flex;
    justify-content: center;
    gap: 10px;
    margin-top: 8px;
  }

  .event-actions button {
    padding: 4px 8px;
    font-size: 12px;
    background-color: burlywood;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.2s ease;
  }

  .event-actions button:hover {
    background-color: peru;
  }

  .age-text {
    font-weight: bold;
    font-size: 14px;
  }

  .year-text {
    font-size: 12px;
    color: #555;
  }
</style>
