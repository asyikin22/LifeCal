<script>
  import * as THREE from 'three';
  import { onMount } from 'svelte';

  let container;
  let scene, camera, renderer, animationId;
  const waveSpeed = 0.002;
  let columns = [];
  const columnCount = 20;

  // Sea creature emojis
  const seaCreatures = ['🐠', '🐡', '🦀', '🐙', '🪸', '🐚', '⭐'];

  // Generate random sea creatures
  const fishCount = 120;
  const fishes = Array.from({ length: fishCount }, () => ({
    top: Math.random() * 100,
    left: Math.random() * 100,
    delay: Math.random() * 5,
    duration: 2 + Math.random() * 3, // 2s to 5s float speed
    amplitude: 5 + Math.random() * 10, // float height
    icon: seaCreatures[Math.floor(Math.random() * seaCreatures.length)]
  }));

  onMount(() => {
    init();
    animate();
    window.addEventListener('resize', onWindowResize);

    return () => {
      cancelAnimationFrame(animationId);
      window.removeEventListener('resize', onWindowResize);

      if (renderer) {
        renderer.dispose();
        if (renderer.domElement && renderer.domElement.parentNode) {
          renderer.domElement.parentNode.removeChild(renderer.domElement);
        }
        renderer = null;
      }

      columns = []; // Clear columns array to avoid stale data
    };
  });

  function init() {
    scene = new THREE.Scene();
    scene.background = new THREE.Color(0x001d3d); // Deep ocean background

    camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    camera.position.z = 20;

    renderer = new THREE.WebGLRenderer({ alpha: true });
    renderer.setSize(window.innerWidth, window.innerHeight);
    container.appendChild(renderer.domElement);

    const light = new THREE.DirectionalLight(0xffffff, 1);
    light.position.set(0, 1, 1);
    scene.add(light);

    const colors = [0x001f3f, 0x0074D9]; // Alternating dark and light blue
    const totalWidth = 100;
    const columnWidth = totalWidth / columnCount;

    columns = []; // Clear in case of re-init

    for (let i = 0; i < columnCount; i++) {
      const geometry = new THREE.PlaneGeometry(columnWidth, 60, 50, 50);
      const material = new THREE.MeshPhongMaterial({
        color: colors[i % 2],
        side: THREE.DoubleSide,
        flatShading: true,
        transparent: true,
        opacity: 0.6
      });

      const mesh = new THREE.Mesh(geometry, material);
      mesh.position.x = -totalWidth / 2 + columnWidth / 2 + i * columnWidth;
      scene.add(mesh);

      columns.push({
        mesh,
        initialPositions: geometry.attributes.position.array.slice(),
        positions: geometry.attributes.position,
        offset: i * 1000
      });
    }
  }

  function animate() {
    animationId = requestAnimationFrame(animate);

    if (!columns || columns.length === 0) return;

    const time = Date.now() * waveSpeed;

    for (const col of columns) {
      if (!col.positions || !col.initialPositions) continue;

      for (let i = 0; i < col.positions.count; i++) {
        const ix = i * 3;
        const y = col.initialPositions[ix + 1];
        col.positions.array[ix + 2] = Math.sin(y * 0.5 + time + col.offset) * 0.8;
      }
      col.positions.needsUpdate = true;
    }

    renderer.render(scene, camera);
  }

  function onWindowResize() {
    camera.aspect = window.innerWidth / window.innerHeight;
    camera.updateProjectionMatrix();
    renderer.setSize(window.innerWidth, window.innerHeight);
  }
</script>

<div bind:this={container} class="background"></div>

<div class="ocean">
  {#each fishes as fish}
    <div 
      class="fish" 
      style="
        top: {fish.top}%; 
        left: {fish.left}%; 
        animation-delay: {fish.delay}s;
        animation-duration: {fish.duration}s;
        --amplitude: -{fish.amplitude}px;">
      {fish.icon}
    </div>
  {/each}
</div>

<style>
  .background {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -2;
  }

  .ocean {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    overflow: hidden;
    z-index: -1;
    pointer-events: none;
  }

  .fish {
    position: absolute;
    font-size: 14px;
    animation-name: float;
    animation-timing-function: ease-in-out;
    animation-iteration-count: infinite;
  }

  @keyframes float {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(var(--amplitude)); }
  }
</style>
