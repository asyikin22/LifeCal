<script>
  import { Lightbulb, Github, Mail, Coffee } from 'lucide-svelte';
</script>

<div class="sidebar">
  
  <a href="https://waitbutwhy.com/2014/05/life-weeks.html" target="_blank" class="icon" title="Inspiration">
    <Lightbulb size="30" />
  </a>

  <a href="https://github.com/your-repo" target="_blank" class="icon" title="View GitHub">
    <Github size="30" />
  </a>

  <a href="mailto:your@email.com" class="icon" title="Email Me">
    <Mail size="30" />
  </a>

  <a href="https://www.buymeacoffee.com/yourprofile" target="_blank" class="icon" title="Buy Me a Coffee">
    <Coffee size="30" />
  </a>

</div>


<style>
.sidebar {
  position: fixed;
  top: 35%;
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
  display: flex;
  justify-content: center;
  align-items: center;
  transition: background-color 0.3s ease;
  backdrop-filter: blur(5px); /* subtle frosted effect */
}

.icon:hover {
  background-color: rgba(255, 255, 255, 0.9);
  cursor: pointer;
}

</style>
