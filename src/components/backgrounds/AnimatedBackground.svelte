<script>
  import { onMount } from 'svelte';
  import * as THREE from 'three';

  let container;

  onMount(() => {
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(
      75,
      window.innerWidth / window.innerHeight,
      0.1,
      1000
    );

    const renderer = new THREE.WebGLRenderer({ alpha: true, antialias: true });
    renderer.setSize(window.innerWidth, window.innerHeight);
    renderer.setClearColor(0x1a1a2e, 1); 
    container.appendChild(renderer.domElement);

    // Create Circular Particle Texture
    const canvas = document.createElement('canvas');
    canvas.width = 64;
    canvas.height = 64;
    const ctx = canvas.getContext('2d');
    const gradient = ctx.createRadialGradient(32, 32, 0, 32, 32, 32);
    gradient.addColorStop(0, 'white');
    gradient.addColorStop(1, 'transparent');
    ctx.fillStyle = gradient;
    ctx.fillRect(0, 0, 64, 64);

    const texture = new THREE.CanvasTexture(canvas);

    const particlesCount = 2000;
    const geometry = new THREE.BufferGeometry();
    const positions = new Float32Array(particlesCount * 3);
    const colors = new Float32Array(particlesCount * 3);

    const color = new THREE.Color();

    for (let i = 0; i < particlesCount; i++) {
      const radius = Math.random() * 60;
      const angle = Math.random() * 2 * Math.PI;
      const branch = (i % 5) * (2 * Math.PI / 5);

      positions[i * 3] = Math.cos(angle + branch) * radius;
      positions[i * 3 + 1] = (Math.random() - 0.5) * 50;
      positions[i * 3 + 2] = Math.sin(angle + branch) * radius;

      // Random color for each particle
      color.setHSL(Math.random(), 1.0, 0.6);
      colors[i * 3] = color.r;
      colors[i * 3 + 1] = color.g;
      colors[i * 3 + 2] = color.b;
    }

    geometry.setAttribute('position', new THREE.BufferAttribute(positions, 3));
    geometry.setAttribute('color', new THREE.BufferAttribute(colors, 3));

    const material = new THREE.PointsMaterial({
      size: 0.7,
      map: texture,
      transparent: true,
      alphaTest: 0.5,
      depthWrite: false,
      vertexColors: true,
      blending: THREE.AdditiveBlending
    });

    const galaxy = new THREE.Points(geometry, material);
    scene.add(galaxy);

    camera.position.z = 10;

    function animate() {
      requestAnimationFrame(animate);
      galaxy.rotation.y += 0.0007;
      renderer.render(scene, camera);
    }

    animate();

    window.addEventListener('resize', () => {
      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(window.innerWidth, window.innerHeight);
    });
  });
</script>

<div bind:this={container} class="bg-container"></div>

<style>
  .bg-container {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -1;
    overflow: hidden;
    pointer-events: none;
  }
</style>
