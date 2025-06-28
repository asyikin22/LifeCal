<script>
  export let birthYear;
  export let age;
  export let updateAge;

  let dateString = '';
  let timeString = '';
  let country = '';

  function updateTime() {
    const now = new Date();
    
    const timezone = Intl.DateTimeFormat().resolvedOptions().timeZone;
    
    // Extract country part from timezone, if possible
    const parts = timezone.split('/');
    country = parts[1] ? parts[1].replace(/_/g, ' ') : timezone;

    dateString = now.toLocaleDateString(undefined, {
      weekday: 'long',
      year: 'numeric',
      month: 'long',
      day: 'numeric'
    });

    timeString = now.toLocaleTimeString(undefined, {
      hour: '2-digit',
      minute: '2-digit',
      second: '2-digit'
    });
  }

  setInterval(updateTime, 1000);
  updateTime();
</script>

<div class="top-bar">
  
  <div class="box">
    <h3 class="title">Life Calendar</h3>
    <div class="age-text">
        {#if age !== null}
        You are {age} years young! 😊
        {/if}
    </div>
  </div>

  <div class="box">
    <label for="birthYearInput">Birth Year:</label>
    <input 
      id="birthYearInput" 
      type="number" 
      bind:value={birthYear} 
      placeholder="e.g. 1990" 
      on:keydown={(e) => e.key === 'Enter' && updateAge(birthYear)}
    />
    <button on:click={() => updateAge(birthYear)}>Generate Calendar</button>
  </div>

  <div class="box">
    <div style="margin-top: 2px; font-size: 30px;">{country}</div>
    <div style="font-size: 18px; margin-top: 5px;">{dateString}</div>
    <div style="font-size: 16px; margin-top: 5px; font-weight: bold;">{timeString}</div>
  </div>

</div>

<style>
  .top-bar {
    display: flex;
    justify-content: center;
    align-items: flex-start;
    flex-wrap: wrap;
    gap: 10px;
    margin-bottom: 20px;
  }

  .box {
      background-color: #f9f9f9;
      border: 1px solid #ccc; 
      border-radius: 8px;
      padding: 15px;
      width: 240px;
      height: 120px;
      text-align: center;
      box-shadow: 2px 2px 6px rgba(0,0,0,0.1);
      box-sizing: border-box;
      margin-top: 20px;
      display: flex;
      flex-direction: column;
      justify-content: center; /* Vertically center */
      align-items: center;     /* Horizontally center */
      text-align: center;
  }

  input, button {
    width: 100%;
    margin-top: 6px;
    padding: 6px;
    box-sizing: border-box;
    text-align: center;
  }

  .title {
    margin: 2px 0 4px 0; /* Top 2px, bottom 4px for tighter spacing */
    font-size: 31px;  
  }

  .age-text {
    font-size: 18px;
 }

</style>
