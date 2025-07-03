<script>
  import { Lightbulb, Github, Mail, Coffee } from 'lucide-svelte';
</script>

<div class="sidebar">
  
  <a href="https://waitbutwhy.com/2014/05/life-weeks.html" target="_blank" class="icon" title="Inspiration">
    <Lightbulb size="35" />
  </a>

  <a href="https://github.com/asyikin22/LifeCal.git" target="_blank" class="icon" title="My GitHub">
    <Github size="35" />
  </a>

  <a href="mailto:your@email.com" class="icon" title="Email Me">
    <Mail size="35" />
  </a>

  <a href="https://www.buymeacoffee.com/yourprofile" target="_blank" class="icon" title="Buy Me a Coffee">
    <Coffee size="35" />
  </a>

</div>


<style>
.sidebar {
  position: fixed;
  top: 32%;
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

</style>
