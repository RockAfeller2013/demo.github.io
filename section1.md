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

# WebGL Test

<canvas id="glcanvas" width="400" height="300"></canvas>

{% raw %}
<script>
  const canvas = document.getElementById("glcanvas");
  const gl = canvas.getContext("webgl");

  if (!gl) {
    alert("WebGL not supported");
  } else {
    gl.clearColor(0.0, 0.0, 0.0, 1.0);
    gl.clear(gl.COLOR_BUFFER_BIT);
  }
</script>
{% endraw %}

<canvas id="glcanvas" width="800" height="450"></canvas>

<script>
  const canvas = document.getElementById("glcanvas");
  const gl = canvas.getContext("webgl");

  if (!gl) {
    alert("WebGL not supported in this browser");
  }

  // Vertex shader program
  const vsSource = `
    attribute vec4 aVertexPosition;
    attribute vec4 aVertexColor;
    uniform mat4 uModelViewMatrix;
    uniform mat4 uProjectionMatrix;
    varying lowp vec4 vColor;
    void main(void) {
      gl_Position = uProjectionMatrix * uModelViewMatrix * aVertexPosition;
      vColor = aVertexColor;
    }
  `;

  // Fragment shader program
  const fsSource = `
    varying lowp vec4 vColor;
    void main(void) {
      gl_FragColor = vColor;
    }
  `;

  function loadShader(type, source) {
    const shader = gl.createShader(type);
    gl.shaderSource(shader, source);
    gl.compileShader(shader);
    return shader;
  }

  const vertexShader = loadShader(gl.VERTEX_SHADER, vsSource);
  const fragmentShader = loadShader(gl.FRAGMENT_SHADER, fsSource);

  const shaderProgram = gl.createProgram();
  gl.attachShader(shaderProgram, vertexShader);
  gl.attachShader(shaderProgram, fragmentShader);
  gl.linkProgram(shaderProgram);

  const programInfo = {
    program: shaderProgram,
    attribLocations: {
      vertexPosition: gl.getAttribLocation(shaderProgram, 'aVertexPosition'),
      vertexColor: gl.getAttribLocation(shaderProgram, 'aVertexColor'),
    },
    uniformLocations: {
      projectionMatrix: gl.getUniformLocation(shaderProgram, 'uProjectionMatrix'),
      modelViewMatrix: gl.getUniformLocation(shaderProgram, 'uModelViewMatrix'),
    },
  };

  const positions = [
    // Front face
    -1.0, -1.0,  1.0,
     1.0, -1.0,  1.0,
     1.0,  1.0,  1.0,
    -1.0,  1.0,  1.0,
    // Back face
    -1.0, -1.0, -1.0,
    -1.0,  1.0, -1.0,
     1.0,  1.0, -1.0,
     1.0, -1.0, -1.0,
  ];

  const positionBuffer = gl.createBuffer();
  gl.bindBuffer(gl.ARRAY_BUFFER, positionBuffer);
  gl.bufferData(gl.ARRAY_BUFFER, new Float32Array(positions), gl.STATIC_DRAW);

  const faceColors = [
    [1.0, 0.0, 0.0, 1.0], // Front: Red
    [0.0, 1.0, 0.0, 1.0], // Back: Green
    [0.0, 0.0, 1.0, 1.0], // Top: Blue
    [1.0, 1.0, 0.0, 1.0], // Bottom: Yellow
    [1.0, 0.0, 1.0, 1.0], // Right: Magenta
    [0.0, 1.0, 1.0, 1.0], // Left: Cyan
  ];

  let colors = [];
  for (let j = 0; j < faceColors.length; ++j) {
    const c = faceColors[j];
    colors = colors.concat(c, c, c, c);
  }

  const colorBuffer = gl.createBuffer();
  gl.bindBuffer(gl.ARRAY_BUFFER, colorBuffer);
  gl.bufferData(gl.ARRAY_BUFFER, new Float32Array(colors), gl.STATIC_DRAW);

  const indices = [
    0, 1, 2, 0, 2, 3, // front
    4, 5, 6, 4, 6, 7, // back
    5, 3, 2, 5, 2, 6, // top
    4, 7, 1, 4, 1, 0, // bottom
    7, 6, 2, 7, 2, 1, // right
    4, 0, 3, 4, 3, 5, // left
  ];

  const indexBuffer = gl.createBuffer();
  gl.bindBuffer(gl.ELEMENT_ARRAY_BUFFER, indexBuffer);
  gl.bufferData(gl.ELEMENT_ARRAY_BUFFER, new Uint16Array(indices), gl.STATIC_DRAW);

  let cubeRotation = 0.0;

  function drawScene() {
    gl.clearColor(0, 0, 0, 1);
    gl.clearDepth(1.0);
    gl.enable(gl.DEPTH_TEST);
    gl.depthFunc(gl.LEQUAL);

    gl.clear(gl.COLOR_BUFFER_BIT | gl.DEPTH_BUFFER_BIT);

    const fieldOfView = 45 * Math.PI / 180;
    const aspect = canvas.clientWidth / canvas.clientHeight;
    const projectionMatrix = mat4.create();
    mat4.perspective(projectionMatrix, fieldOfView, aspect, 0.1, 100.0);

    const modelViewMatrix = mat4.create();
    mat4.translate(modelViewMatrix, modelViewMatrix, [0, 0, -6]);
    mat4.rotate(modelViewMatrix, modelViewMatrix, cubeRotation, [0, 0, 1]);
    mat4.rotate(modelViewMatrix, modelViewMatrix, cubeRotation * 0.7, [0, 1, 0]);

    {
      const numComponents = 3;
      const type = gl.FLOAT;
      const normalize = false;
      const stride = 0;
      const offset = 0;
      gl.bindBuffer(gl.ARRAY_BUFFER, positionBuffer);
      gl.vertexAttribPointer(programInfo.attribLocations.vertexPosition, numComponents, type, normalize, stride, offset);
      gl.enableVertexAttribArray(programInfo.attribLocations.vertexPosition);
    }

    {
      const numComponents = 4;
      const type = gl.FLOAT;
      const normalize = false;
      const stride = 0;
      const offset = 0;
      gl.bindBuffer(gl.ARRAY_BUFFER, colorBuffer);
      gl.vertexAttribPointer(programInfo.attribLocations.vertexColor, numComponents, type, normalize, stride, offset);
      gl.enableVertexAttribArray(programInfo.attribLocations.vertexColor);
    }

    gl.bindBuffer(gl.ELEMENT_ARRAY_BUFFER, indexBuffer);

    gl.useProgram(programInfo.program);

    gl.uniformMatrix4fv(programInfo.uniformLocations.projectionMatrix, false, projectionMatrix);
    gl.uniformMatrix4fv(programInfo.uniformLocations.modelViewMatrix, false, modelViewMatrix);

    const vertexCount = 36;
    const type = gl.UNSIGNED_SHORT;
    gl.drawElements(gl.TRIANGLES, vertexCount, type, 0);
  }

  function render() {
    cubeRotation += 0.01;
    drawScene();
    requestAnimationFrame(render);
  }

  // Load gl-matrix library inline
  const mat4 = {
    create: () => new Float32Array(16).fill(0).map((v, i) => (i % 5 == 0 ? 1 : 0)),
    perspective: (out, fovy, aspect, near, far) => {
      const f = 1.0 / Math.tan(fovy / 2);
      out[0] = f / aspect;
      out[5] = f;
      out[10] = (far + near) / (near - far);
      out[11] = -1;
      out[14] = (2 * far * near) / (near - far);
    },
    translate: (out, a, v) => {
      out[12] += v[0];
      out[13] += v[1];
      out[14] += v[2];
    },
    rotate: (out, a, rad, axis) => {
      const x = axis[0], y = axis[1], z = axis[2];
      const len = Math.hypot(x, y, z);
      if (len < 0.000001) return null;
      const s = Math.sin(rad);
      const c = Math.cos(rad);
      const t = 1 - c;
      const a00 = out[0], a01 = out[1], a02 = out[2], a03 = out[3];
      const a10 = out[4], a11 = out[5], a12 = out[6], a13 = out[7];
      const a20 = out[8], a21 = out[9], a22 = out[10], a23 = out[11];
      const b00 = x * x * t + c, b01 = y * x * t + z * s, b02 = z * x * t - y * s;
      const b10 = x * y * t - z * s, b11 = y * y * t + c, b12 = z * y * t + x * s;
      const b20 = x * z * t + y * s, b21 = y * z * t - x * s, b22 = z * z * t + c;
      out[0] = a00 * b00 + a10 * b01 + a20 * b02;
      out[1] = a01 * b00 + a11 * b01 + a21 * b02;
      out[2] = a02 * b00 + a12 * b01 + a22 * b02;
      out[3] = a03 * b00 + a13 * b01 + a23 * b02;
      out[4] = a00 * b10 + a10 * b11 + a20 * b12;
      out[5] = a01 * b10 + a11 * b11 + a21 * b12;
      out[6] = a02 * b10 + a12 * b11 + a22 * b12;
      out[7] = a03 * b10 + a13 * b11 + a23 * b12;
      out[8] = a00 * b20 + a10 * b21 + a20 * b22;
      out[9] = a01 * b20 + a11 * b21 + a21 * b22;
      out[10] = a02 * b20 + a12 * b21 + a22 * b22;
      out[11] = a03 * b20 + a13 * b21 + a23 * b22;
    },
  };

  render();
</script>


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


