<script>
  import TopBar from '../components/TopBar.svelte';
  import LifeGrid from '../components/LifeGrid.svelte';
  import AnimatedBackground from '../components/AnimatedBackground.svelte';
  import SocialBar from '../components/SocialBar.svelte';

  let birthYear = '';
  let age = null;

  function updateAge(year) {
    const currentYear = new Date().getFullYear();
    age = year ? currentYear - year : null;
  }
</script>

<!-- TOP BAR at the top -->
<TopBar bind:birthYear bind:age {updateAge} />

<!-- PROGRESS BAR in the middle -->
{#if age !== null}
  <div class="progress-bar-container">
    <div class="progress-bar" style="width: {Math.min((age / 50) * 100, 100)}%"></div>
    <div class="progress-text">
      {Math.min(Math.round((age / 50) * 100), 100)}% of your 50-year journey completed!
    </div>
  </div>
{/if}

<AnimatedBackground />
<SocialBar />

<!-- LIFE GRID below -->
<LifeGrid {birthYear} />

<style>
  .progress-bar-container {
    width: 70%;
    background-color: #e0cdcd;
    border-radius: 8px;
    margin: 15px auto;
    overflow: hidden;
    height: 25px;
    position: relative;
    margin-bottom: 0;
  }

  .progress-bar {
    background-color: #77ed7b;
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
    color: #333;
    font-weight: bold;
  }
</style>

