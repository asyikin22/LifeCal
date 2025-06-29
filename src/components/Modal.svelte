<script>
  export let show = false;
  export let title = '';
  export let onClose;
</script>

{#if show}
  <div class="overlay" on:click={onClose}>
    <div class="modal" on:click|stopPropagation>
      <h3>{title}</h3>
      <slot />
      <button class="close-btn" on:click={onClose}>Close</button>
    </div>
  </div>
{/if}

<style>
  .overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0,0,0,0.4);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
  }

  .modal {
    background: white;
    padding: 20px;
    border-radius: 8px;
    min-width: 300px;
    max-width: 90%;
    box-shadow: 0 2px 10px rgba(0,0,0,0.3);
    position: relative;
  }

  .close-btn {
    margin-top: 12px;
    width: 100%;
    padding: 8px;
    cursor: pointer;
  }
</style>
