<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Real-time Hand Tracking Energy Orb</title>
  <!-- MediaPipe dependencies -->
  <script src="https://cdn.jsdelivr.net/npm/@mediapipe/camera_utils/camera_utils.js" crossorigin="anonymous"></script>
  <script src="https://cdn.jsdelivr.net/npm/@mediapipe/hands/hands.js" crossorigin="anonymous"></script>
  <!-- Three.js for 3D visual effects -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>

  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }
    body {
      background-color: #050508;
      overflow: hidden;
      font-family: Arial, sans-serif;
    }
    #container {
      position: relative;
      width: 100vw;
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
    }
    video {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      object-fit: cover;
      transform: scaleX(-1); /* Mirror camera feed */
      z-index: 1;
    }
    canvas#three-canvas {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      transform: scaleX(-1); /* Mirror WebGL canvas to match camera */
      z-index: 2;
      pointer-events: none;
    }
  </style>
</head>
<body>

  <div id="container">
    <video id="webcam" autoplay playsinline></video>
    <canvas id="three-canvas"></canvas>
  </div>

  <script>
    const videoElement = document.getElementById('webcam');
    const canvasElement = document.getElementById('three-canvas');

    // -----------------------------------------------------------------
    // 1. Three.js Scene Setup
    // -----------------------------------------------------------------
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 0.1, 1000);
    camera.position.z = 5;

    const renderer = new THREE.WebGLRenderer({ canvas: canvasElement, alpha: true, antialias: true });
    renderer.setSize(window.innerWidth, window.innerHeight);
    renderer.setPixelRatio(window.devicePixelRatio);

    window.addEventListener('resize', () => {
      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(window.innerWidth, window.innerHeight);
    });

    // -----------------------------------------------------------------
    // 2. Swirling Particle Sphere (Rasengan Effect)
    // -----------------------------------------------------------------
    function createEnergyOrb() {
      const particleCount = 1500;
      const geometry = new THREE.BufferGeometry();
      const positions = new Float32Array(particleCount * 3);
      const colors = new Float32Array(particleCount * 3);

      const color1 = new THREE.Color(0x00f0ff);
      const color2 = new THREE.Color(0x0044ff);

      for (let i = 0; i < particleCount; i++) {
        // Distribute points on a sphere
        const u = Math.random();
        const v = Math.random();
        const theta = u * 2.0 * Math.PI;
        const phi = Math.acos(2.0 * v - 1.0);
        const radius = 0.6 + Math.random() * 0.2;

        const x = radius * Math.sin(phi) * Math.cos(theta);
        const y = radius * Math.sin(phi) * Math.sin(theta);
        const z = radius * Math.cos(phi);

        positions[i * 3] = x;
        positions[i * 3 + 1] = y;
        positions[i * 3 + 2] = z;

        const mixedColor = color1.clone().lerp(color2, Math.random());
        colors[i * 3] = mixedColor.r;
        colors[i * 3 + 1] = mixedColor.g;
        colors[i * 3 + 2] = mixedColor.b;
      }

      geometry.setAttribute('position', new THREE.BufferAttribute(positions, 3));
      geometry.setAttribute('color', new THREE.BufferAttribute(colors, 3));

      const material = new THREE.PointsMaterial({
        size: 0.08,
        vertexColors: true,
        transparent: true,
        opacity: 0.85,
        blending: THREE.AdditiveBlending
      });

      const particleSystem = new THREE.Points(geometry, material);

      // Core light
      const coreGeo = new THREE.SphereGeometry(0.35, 32, 32);
      const coreMat = new THREE.MeshBasicMaterial({
        color: 0xffffff,
        transparent: true,
        opacity: 0.9,
        blending: THREE.AdditiveBlending
      });
      const coreMesh = new THREE.Mesh(coreGeo, coreMat);

      const orbGroup = new THREE.Group();
      orbGroup.add(particleSystem);
      orbGroup.add(coreMesh);
      orbGroup.visible = false; // Hide until hand detected

      scene.add(orbGroup);
      return orbGroup;
    }

    const orb1 = createEnergyOrb();
    const orb2 = createEnergyOrb();

    // -----------------------------------------------------------------
    // 3. MediaPipe Hands Configuration
    // -----------------------------------------------------------------
    const hands = new Hands({
      locateFile: (file) => `https://cdn.jsdelivr.net/npm/@mediapipe/hands/${file}`
    });

    hands.setOptions({
      maxNumHands: 2,
      modelComplexity: 1,
      minDetectionConfidence: 0.65,
      minTrackingConfidence: 0.65
    });

    hands.onResults((results) => {
      orb1.visible = false;
      orb2.visible = false;

      if (results.multiHandLandmarks && results.multiHandLandmarks.length > 0) {
        results.multiHandLandmarks.forEach((landmarks, index) => {
          const currentOrb = index === 0 ? orb1 : orb2;

          // Landmark 9 corresponds to Middle Finger MCP (Palm Center)
          const palmLandmark = landmarks[9];

          // Map normalized screen coordinates (0 to 1) into Three.js 3D space
          const vector = new THREE.Vector3(
            (palmLandmark.x * 2) - 1,
            -(palmLandmark.y * 2) + 1,
            0.5
          );

          vector.unproject(camera);
          const dir = vector.sub(camera.position).normalize();
          const distance = -camera.position.z / dir.z;
          const pos = camera.position.clone().add(dir.multiplyScalar(distance));

          currentOrb.position.copy(pos);
          currentOrb.visible = true;
        });
      }
    });

    // -----------------------------------------------------------------
    // 4. Camera Pipeline & Render Loop
    // -----------------------------------------------------------------
    const cameraUtils = new Camera(videoElement, {
      onFrame: async () => {
        await hands.send({ image: videoElement });
      },
      width: 1280,
      height: 720
    });
    cameraUtils.start();

    function animate() {
      requestAnimationFrame(animate);

      // Rotate particles continuously for swirl effect
      [orb1, orb2].forEach(orb => {
        if (orb.visible) {
          orb.children[0].rotation.y += 0.05;
          orb.children[0].rotation.x += 0.02;
        }
      });

      renderer.render(scene, camera);
    }
    animate();
  </script>
</body>
</html>
