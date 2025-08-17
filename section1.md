---
title: Section 1 - Youtube example
nav_order: 2
---

# Section 1: Youtube example

Github Pages Jekly Template and Markdown does not support embeding youtube, but you can add this inside the _includes folder. Here are some examples;

Autoplay single video muted:
{% include youtube.html id="dQw4w9WgXcQ" autoplay="1" mute="1" %}


Fixed size autoplay:
{% include youtube.html id="dQw4w9WgXcQ" width="800px" height="450px" autoplay="1" %}

These don't seem to work, playlist is WIP.


<iframe 
  width="800" 
  height="450" 
  src="https://www.youtube.com/embed/videoseries?list=PLiaHhY2iBX9hdHaRr6b7XevZtgZRa1PoU&start=30&autoplay=1&mute=1" 
  title="YouTube playlist" 
  frameborder="0" 
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
  allowfullscreen>
</iframe>

# WebGL doesnt work but three.js works 

<canvas id="cubeCanvas" width="800" height="450"></canvas>

<!-- Load Three.js -->
<script src="https://cdn.jsdelivr.net/npm/three@0.155.0/build/three.min.js"></script>

<script>
  // Scene
  const scene = new THREE.Scene();

  // Camera
  const camera = new THREE.PerspectiveCamera(
    75, 
    800 / 450, 
    0.1, 
    1000
  );
  camera.position.z = 3;

  // Renderer
  const renderer = new THREE.WebGLRenderer({ canvas: document.getElementById("cubeCanvas") });
  renderer.setSize(800, 450);

  // Cube
  const geometry = new THREE.BoxGeometry();
  const material = new THREE.MeshNormalMaterial(); // colorful effect
  const cube = new THREE.Mesh(geometry, material);
  scene.add(cube);

  // Animation loop
  function animate() {
    requestAnimationFrame(animate);
    cube.rotation.x += 0.01;
    cube.rotation.y += 0.01;
    renderer.render(scene, camera);
  }

  animate();
</script>


