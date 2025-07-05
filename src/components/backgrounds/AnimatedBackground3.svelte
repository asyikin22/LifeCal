<script>
  import { onMount } from 'svelte';
  
  const flowers = ['🌸','🌼', '🌻', '🌹', '🥀', '🍂', '🍁', '🌺', '🌷', '🍃','🌿', '🍀'];
  const flowerCount = 120;
  
  const floatingFlowers = Array.from({ length: flowerCount }, () => ({
    left: Math.random() * 100,
    delay: Math.random() * 8, // Increased delay range for more staggered start
    duration: 8 + Math.random() * 20, // Much slower: 8s to 20s fall speed
    size: 12 + Math.random() * 16,   // 12px to 28px
    spinDirection: Math.random() < 0.5 ? 'normal' : 'reverse', // Random spin direction
    icon: flowers[Math.floor(Math.random() * flowers.length)]
  }));

  onMount(() => {
    const garden = document.querySelector(".garden");
    
    if (!garden) {
      console.error('Garden element not found!');
      return;
    }

    floatingFlowers.forEach(flower => {
      const flowerEl = document.createElement("div");
      flowerEl.className = `flower ${flower.spinDirection}`;
      flowerEl.style.left = `${flower.left}%`;
      flowerEl.style.top = `${Math.random() * 120 - 10}%`; // Scatter throughout screen
      flowerEl.style.animationDelay = `${flower.delay - Math.random() * flower.duration}s, ${flower.delay}s, ${flower.delay}s`;
      flowerEl.style.animationDuration = `${flower.duration}s, ${flower.duration/2}s, ${flower.duration/3}s`;
      flowerEl.style.fontSize = `${flower.size}px`;
      flowerEl.textContent = flower.icon;
      garden.appendChild(flowerEl);
    });
  });
</script>

<div class="background"></div>
<div class="garden"></div>

<style>
  .background {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: #f0a5c4;
    z-index: -1;
  }

  .garden {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    overflow: hidden;
    z-index: -1;
    pointer-events: none;
  }

  :global(.flower) {
    position: absolute;
    top: -10%;
    animation-name: fall, rotate, twinkle;
    animation-iteration-count: infinite;
    animation-timing-function: linear, linear, ease-in-out;
    animation-duration: 25s, 4s, 3s; /* Slower default durations */
  }

  :global(.flower.normal) {
    animation-direction: normal, normal, normal;
  }

  :global(.flower.reverse) {
    animation-direction: normal, reverse, normal;
  }

  @keyframes fall {
    0% {
      top: -10%;
    }
    100% {
      top: 110%;
    }
  }

  @keyframes rotate {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
  }

  @keyframes twinkle {
    0%, 100% { opacity: 0.8; }
    50% { opacity: 1; }
  }
</style>