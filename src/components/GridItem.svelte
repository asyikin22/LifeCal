<script>
  import { countryList } from '../countries.js';
  export let age;
  export let year;

  let selectedCountry = '';
  let flag = '';
  let events = [];
  let showEventForm = false;
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
    }
  }

  function confirmAddEvent() {
    if (selectedType && detail) {
      const selected = eventTypes.find(e => e.type === selectedType);
      if (selected) {
        events = [...events, { emoji: selected.emoji, detail, color: selected.color }];
      }
      showEventForm = false;
      selectedType = '';
      detail = '';
    }
  }

  function deleteEvent(index) {
    events = events.filter((_, i) => i !== index);
  }
</script>

<div class="grid-item">
  <div class="age-year">
    <strong>Age {age}</strong> — {year}
    {#if flag}
      <img src={flag} alt="Flag" class="flag" title={selectedCountry} />
    {/if}
  </div>

  {#if !flag}
    <select on:change={handleCountryChange}>
      <option value="">Select country...</option>
      {#each countryList as country}
        <option value={country.code} data-name={country.name}>{country.name}</option>
      {/each}
    </select>
  {/if}

  {#each events as event, i}
    <div class="event-block" style="background-color: {event.color}">
      {event.emoji} {event.detail}
      <button type="button" on:click={() => deleteEvent(i)} class="delete-btn">✖</button>
    </div>
  {/each}

  {#if showEventForm}
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
  {/if}

  <div class="spacer"></div>
  <button class="add-btn" on:click={() => showEventForm = true}>+</button>
</div>

<style>
  .grid-item {
    border: 1px solid #999;
    padding: 6px;
    padding-bottom: 24px;
    font-size: 12px;
    min-height: 140px;
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
  }

  select {
    font-size: 12px;
    width: 100%;
    margin-bottom: 4px;
  }

  .flag {
    width: 20px;
    height: 14px;
    margin-left: 15px;
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
    background: #f1f1f1;
    margin-top: 4px;
    padding: 2px 4px;
    width: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 12px;
    position: relative;
  }

  .delete-btn {
    cursor: pointer;
    margin-left: 6px;
    background: transparent;
    border: none;
    opacity: 0.2;
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
</style>
