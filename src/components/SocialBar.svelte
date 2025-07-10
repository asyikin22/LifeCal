<script>
  import { Lightbulb, NotebookPenIcon, Mail, Coffee, ArrowUp, Palette } from 'lucide-svelte';
  import { backgroundIndex } from '../stores.js';

  function scrollToTop() {
    window.scrollTo({ top: 0, behavior: 'smooth' });
  }

  function changeBackground() {
    backgroundIndex.update(n => (n + 1) % 3); // Cycles 0 → 1 → 2 → 0
  }

</script>

<div class="sidebar">

  <button class="icon" title="Change Background" on:click={changeBackground}>
    <Palette size="35" />
  </button>
  
  <a href="https://waitbutwhy.com/2014/05/life-weeks.html" target="_blank" class="icon" title="Inspiration">
    <Lightbulb size="35" />
  </a>

  <a href="https://github.com/asyikin22/LifeCal.git" target="_blank" class="icon" title="Code Summary">
    <NotebookPenIcon size="35" />
  </a>

  <a href="mailto:your@email.com" class="icon" title="Email Me">
    <Mail size="35" />
  </a>

</div>

 <button class="scroll-top" on:click={() => window.scrollTo({ top: 0, behavior: 'smooth' })}>
  <ArrowUp size="24" />
</button>


<style>
.sidebar {
  position: fixed;
  top: 30%;
  left: 10px;
  display: flex;
  flex-direction: column;
  gap: 15px;
  z-index: 1000;
}

.icon {
  background-color: rgba(244, 239, 239, 0.4); /* Slight transparency */
  padding: 8px;
  border-radius: 50%;
  border-style: none;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: background-color 0.3s ease;
  backdrop-filter: blur(5px); 
  text-decoration: none;
  color: inherit
}

.icon:visited {
  color: inherit;
}
.icon:focus,
.icon:active {
    outline: none;
    color: inherit;
}

.icon:hover {
  background-color: rgba(255, 255, 255, 0.9);
  cursor: pointer;
}

.scroll-top {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background-color: rgba(244, 239, 239, 0.4); /* Slight transparency */
  padding: 10px;
  border-radius: 50%;
  border: none;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  backdrop-filter: blur(5px);
  transition: background-color 0.3s ease, transform 0.2s ease;
  z-index: 1000;
}

.scroll-top:hover {
  background-color: rgba(255, 255, 255, 0.9);
  transform: translateY(-2px);
}

</style>
