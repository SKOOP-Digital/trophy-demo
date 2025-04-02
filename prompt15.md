
current index.html:
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Saginaw Schools Trophy Room</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <!-- Add Three.js and GLTFLoader -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/loaders/GLTFLoader.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.js"></script>
    <style>
        :root {
            --navy: #000080;
            --gold: #FFD700;
            --black: #000000;
            --white: #ffffff;
            --gray: #f5f5f5;
            --dark-gray: #333333;
            --light-gray: #f8f9fa;
            --shadow-color: rgba(0, 0, 0, 0.1);
            --modal-bg: rgba(0, 0, 0, 0.85);
            --transition-speed: 0.3s;
            --border-radius: 10px;
            --card-shadow: 0 4px 6px var(--shadow-color);
            --hover-transform: translateY(-5px);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
        }

        body {
            min-height: 100vh;
            background: var(--light-gray);
            overflow-x: hidden;
            color: var(--dark-gray);
        }

        .header {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            background: var(--navy);
            color: var(--white);
            padding: 1rem;
            z-index: 1000;
            box-shadow: 0 2px 10px var(--shadow-color);
        }

        .header-content {
            max-width: 1200px;
            margin: 0 auto;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .logo {
            font-size: 1.5rem;
            font-weight: bold;
            color: var(--gold);
            text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.2);
        }

        .main-content {
            margin-top: 80px;
            padding: 2rem;
            max-width: 1200px;
            margin-left: auto;
            margin-right: auto;
        }

        .navigation-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 2rem;
            margin-bottom: 2rem;
        }

        .nav-card {
            background: var(--white);
            border-radius: var(--border-radius);
            overflow: hidden;
            transition: all var(--transition-speed) ease;
            box-shadow: var(--card-shadow);
            cursor: pointer;
            position: relative;
            border: 1px solid rgba(0, 0, 0, 0.05);
        }

        .nav-card:hover {
            transform: var(--hover-transform);
            box-shadow: 0 8px 12px var(--shadow-color);
        }

        .nav-card-content {
            padding: 1.5rem;
            background: linear-gradient(135deg, var(--navy) 0%, rgba(0, 0, 128, 0.8) 100%);
            color: var(--white);
            height: 200px;
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
            position: relative;
            overflow: hidden;
        }

        .nav-card-content::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: linear-gradient(to bottom, transparent 0%, rgba(0, 0, 0, 0.6) 100%);
            z-index: 1;
        }

        .nav-card-content > * {
            position: relative;
            z-index: 2;
        }

        .nav-card h2 {
            font-size: 1.5rem;
            margin-bottom: 0.5rem;
            color: var(--gold);
            text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
        }

        .nav-card p {
            font-size: 0.9rem;
            opacity: 0.9;
        }

        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: var(--modal-bg);
            z-index: 2000;
            backdrop-filter: blur(5px);
        }

        .modal-content {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: var(--white);
            padding: 2.5rem;
            border-radius: var(--border-radius);
            width: 90%;
            max-width: 1000px;
            max-height: 90vh;
            overflow-y: auto;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
        }

        .close-modal {
            position: absolute;
            top: 1rem;
            right: 1rem;
            background: none;
            border: none;
            color: var(--dark-gray);
            font-size: 1.5rem;
            cursor: pointer;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all var(--transition-speed) ease;
        }

        .close-modal:hover {
            background: var(--gray);
            transform: rotate(90deg);
        }

        .trophy-display {
            perspective: 1000px;
            margin: 2rem 0;
            width: 100%;
            height: 500px;
            position: relative;
            background: var(--light-gray);
            border-radius: var(--border-radius);
            overflow: hidden;
            box-shadow: inset 0 0 20px var(--shadow-color);
        }

        .trophy-container {
            width: 100%;
            height: 100%;
            position: relative;
        }

        #trophy-canvas {
            width: 100%;
            height: 100%;
            outline: none;
        }

        .trophy-controls {
            position: absolute;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 15px;
            background: rgba(255, 255, 255, 0.9);
            padding: 12px;
            border-radius: 30px;
            z-index: 1;
            box-shadow: 0 4px 12px var(--shadow-color);
        }

        .trophy-control-btn {
            background: var(--navy);
            border: none;
            color: var(--gold);
            width: 45px;
            height: 45px;
            border-radius: 50%;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all var(--transition-speed) ease;
            box-shadow: 0 2px 5px var(--shadow-color);
        }

        .trophy-control-btn:hover {
            background: var(--gold);
            color: var(--navy);
            transform: translateY(-2px);
        }

        .team-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 1.5rem;
            margin-top: 2rem;
        }

        .player-card {
            background: var(--white);
            padding: 1.5rem;
            border-radius: var(--border-radius);
            cursor: pointer;
            transition: all var(--transition-speed) ease;
            box-shadow: var(--card-shadow);
            border: 1px solid rgba(0, 0, 0, 0.05);
        }

        .player-card:hover {
            background: var(--gold);
            color: var(--navy);
            transform: translateY(-3px);
            box-shadow: 0 6px 12px var(--shadow-color);
        }

        .footer {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background: var(--navy);
            color: var(--white);
            padding: 1rem;
            text-align: center;
            box-shadow: 0 -2px 10px var(--shadow-color);
        }

        .breadcrumb {
            margin-bottom: 2rem;
            color: var(--dark-gray);
            padding: 0.5rem 1rem;
            background: var(--white);
            border-radius: var(--border-radius);
            box-shadow: var(--card-shadow);
            display: inline-flex;
            align-items: center;
        }

        .breadcrumb span {
            margin: 0 0.5rem;
            color: var(--navy);
            font-weight: 500;
        }

        .loading-spinner {
            display: none;
            width: 50px;
            height: 50px;
            border: 5px solid var(--gray);
            border-top: 5px solid var(--navy);
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin: 2rem auto;
        }

        .trophy-loading {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            text-align: center;
            background: rgba(255, 255, 255, 0.9);
            padding: 1.5rem 2rem;
            border-radius: var(--border-radius);
            box-shadow: 0 4px 12px var(--shadow-color);
        }

        .trophy-error {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            text-align: center;
            color: #ff3333;
            background: rgba(255, 255, 255, 0.95);
            padding: 1.5rem;
            border-radius: var(--border-radius);
            width: 80%;
            box-shadow: 0 4px 12px rgba(255, 0, 0, 0.1);
            border: 1px solid rgba(255, 0, 0, 0.2);
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        @media (max-width: 768px) {
            .navigation-grid {
                grid-template-columns: 1fr;
            }

            .modal-content {
                width: 95%;
                padding: 1.5rem;
            }

            .header-content {
                flex-direction: column;
                text-align: center;
                gap: 1rem;
            }

            .trophy-display {
                height: 400px;
            }

            .team-grid {
                grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            }
        }
    </style>
</head>
<body>
    <header class="header">
        <div class="header-content">
            <div class="logo">Saginaw Schools Trophy Room</div>
            <nav class="main-nav">
                <!-- Navigation will be dynamically populated -->
            </nav>
        </div>
    </header>

    <main class="main-content">
        <div class="breadcrumb">
            Home
        </div>

        <div class="navigation-grid">
            <!-- Will be dynamically populated -->
        </div>

        <div class="loading-spinner"></div>
    </main>

    <div class="modal" id="detailModal">
        <div class="modal-content">
            <button class="close-modal">&times;</button>
            <div class="modal-body">
                <!-- Will be dynamically populated -->
            </div>
        </div>
    </div>

    <footer class="footer">
        <p>&copy; 2024 Saginaw Schools Trophy Room. All rights reserved.</p>
    </footer>

    <script>
        let currentData = null;
        let navigationHistory = [];

        // Fetch and load the JSON data
        async function loadData() {
            try {
                const response = await fetch('data.json');
                currentData = await response.json();
                if (!currentData || !currentData.schools || !currentData.sportCategories) {
                    throw new Error('Invalid data structure');
                }
                renderMainNavigation();
            } catch (error) {
                console.error('Error loading data:', error);
                document.querySelector('.navigation-grid').innerHTML = `
                    <div style="text-align: center; color: var(--dark-gray);">
                        <h2>Error loading data</h2>
                        <p>Please try refreshing the page</p>
                    </div>
                `;
            }
        }

        function renderMainNavigation() {
            const grid = document.querySelector('.navigation-grid');
            if (!grid) return;
            grid.innerHTML = '';

            if (navigationHistory.length === 0 && currentData?.schools) {
                currentData.schools.forEach(school => {
                    const card = createNavigationCard(school.name, 'School', () => {
                        navigationHistory.push({ type: 'school', id: school.id });
                        renderSchoolView(school);
                    });
                    grid.appendChild(card);
                });
            }
        }

        function createNavigationCard(title, type, onClick) {
            const card = document.createElement('div');
            card.className = 'nav-card';
            card.innerHTML = `
                <div class="nav-card-content">
                    <h2>${title}</h2>
                    <p>${type}</p>
                </div>
            `;
            card.addEventListener('click', onClick);
            return card;
        }

        function renderSchoolView(school) {
            const grid = document.querySelector('.navigation-grid');
            if (!grid || !currentData?.sportCategories) return;
            grid.innerHTML = '';

            // Render sports categories
            currentData.sportCategories.forEach(sport => {
                const card = createNavigationCard(sport, 'Sport', () => {
                    navigationHistory.push({ type: 'sport', name: sport });
                    renderSportView(school, sport);
                });
                grid.appendChild(card);
            });

            // Add D1 Athletes card
            if (school?.athletes?.d1?.length > 0) {
                const d1Card = createNavigationCard('D1 Athletes', 'Athletes', () => {
                    showAthletesList(school.athletes.d1);
                });
                grid.appendChild(d1Card);
            }

            // Add Pro Athletes card
            if (school?.athletes?.pro?.length > 0) {
                const proCard = createNavigationCard('Pro Athletes', 'Athletes', () => {
                    showAthletesList(school.athletes.pro);
                });
                grid.appendChild(proCard);
            }

            updateBreadcrumb();
        }

        function renderSportView(school, sportName) {
            const grid = document.querySelector('.navigation-grid');
            grid.innerHTML = '';

            const sport = Object.values(school.sports).find(s => s.name === sportName);
            if (sport) {
                Object.entries(sport.years).forEach(([year, yearData]) => {
                    const card = createNavigationCard(year, yearData.achievement, () => {
                        showTeamDetail(school, sport, year, yearData);
                    });
                    grid.appendChild(card);
                });
            }

            updateBreadcrumb();
        }

        // Trophy viewer class
        class TrophyViewer {
            constructor(containerId) {
                this.container = document.getElementById(containerId);
                this.setupLoadingState();
                
                // Check WebGL support
                try {
                    this.renderer = new THREE.WebGLRenderer({ 
                        antialias: true,
                        alpha: true,
                        powerPreference: "high-performance"
                    });
                    this.renderer.physicallyCorrectLights = true;
                    this.renderer.toneMapping = THREE.ACESFilmicToneMapping;
                    this.renderer.toneMappingExposure = 1.0;
                    this.renderer.outputEncoding = THREE.sRGBEncoding;
                    this.renderer.shadowMap.enabled = true;
                    this.renderer.shadowMap.type = THREE.PCFSoftShadowMap;
                } catch (e) {
                    this.showError('WebGL is not supported in your browser');
                    return;
                }

                this.scene = new THREE.Scene();
                this.scene.background = new THREE.Color(0xf8f9fa);
                this.camera = new THREE.PerspectiveCamera(45, this.container.clientWidth / this.container.clientHeight, 0.1, 1000);
                
                // Set up renderer
                this.renderer.setSize(this.container.clientWidth, this.container.clientHeight);
                this.renderer.setPixelRatio(window.devicePixelRatio);
                this.container.appendChild(this.renderer.domElement);

                // Set up camera position
                this.camera.position.set(5, 3, 5);
                this.camera.lookAt(0, 0, 0);

                // Add lights
                this.setupLighting();

                // Add environment map
                this.setupEnvironment();

                // Add controls
                this.controls = new THREE.OrbitControls(this.camera, this.renderer.domElement);
                this.controls.enableDamping = true;
                this.controls.dampingFactor = 0.05;
                this.controls.screenSpacePanning = false;
                this.controls.minDistance = 3;
                this.controls.maxDistance = 10;
                this.controls.maxPolarAngle = Math.PI / 1.5;
                this.controls.target.set(0, 0, 0);

                // Handle window resize
                window.addEventListener('resize', () => this.onWindowResize(), false);

                // Start animation loop
                this.animate();
            }

            setupLighting() {
                // Ambient light
                const ambientLight = new THREE.AmbientLight(0xffffff, 0.5);
                this.scene.add(ambientLight);

                // Main directional light (sun)
                const mainLight = new THREE.DirectionalLight(0xffffff, 1.0);
                mainLight.position.set(5, 5, 5);
                mainLight.castShadow = true;
                mainLight.shadow.mapSize.width = 2048;
                mainLight.shadow.mapSize.height = 2048;
                mainLight.shadow.camera.near = 0.5;
                mainLight.shadow.camera.far = 500;
                mainLight.shadow.bias = -0.0001;
                this.scene.add(mainLight);

                // Fill light
                const fillLight = new THREE.DirectionalLight(0xffffff, 0.4);
                fillLight.position.set(-5, 0, -5);
                this.scene.add(fillLight);

                // Top light
                const topLight = new THREE.DirectionalLight(0xffffff, 0.3);
                topLight.position.set(0, 10, 0);
                this.scene.add(topLight);

                // Ground reflection
                const groundReflection = new THREE.HemisphereLight(
                    0xffffff, // sky color
                    0xffffff, // ground color
                    0.3 // intensity
                );
                this.scene.add(groundReflection);
            }

            setupEnvironment() {
                // Create a subtle gradient background
                const vertexShader = `
                    varying vec2 vUv;
                    void main() {
                        vUv = uv;
                        gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
                    }
                `;

                const fragmentShader = `
                    varying vec2 vUv;
                    void main() {
                        vec3 topColor = vec3(0.95, 0.95, 0.95);
                        vec3 bottomColor = vec3(0.85, 0.85, 0.85);
                        gl_FragColor = vec4(mix(bottomColor, topColor, vUv.y), 1.0);
                    }
                `;

                const uniforms = {};
                const backgroundMaterial = new THREE.ShaderMaterial({
                    vertexShader,
                    fragmentShader,
                    uniforms,
                    side: THREE.BackSide
                });

                const backgroundGeometry = new THREE.SphereGeometry(100, 32, 32);
                const background = new THREE.Mesh(backgroundGeometry, backgroundMaterial);
                this.scene.add(background);

                // Add a subtle ground plane
                const groundGeometry = new THREE.PlaneGeometry(100, 100);
                const groundMaterial = new THREE.ShadowMaterial({ opacity: 0.3 });
                const ground = new THREE.Mesh(groundGeometry, groundMaterial);
                ground.rotation.x = -Math.PI / 2;
                ground.position.y = -2;
                ground.receiveShadow = true;
                this.scene.add(ground);
            }

            setupLoadingState() {
                this.loadingElement = document.createElement('div');
                this.loadingElement.className = 'trophy-loading';
                this.loadingElement.innerHTML = `
                    <div class="loading-spinner"></div>
                    <p>Loading trophy model...</p>
                `;
                this.container.appendChild(this.loadingElement);
            }

            showError(message, details = '') {
                console.error('Trophy Viewer Error:', message, details);
                const errorElement = document.createElement('div');
                errorElement.className = 'trophy-error';
                errorElement.innerHTML = `
                    <h3>Error Loading Trophy</h3>
                    <p>${message}</p>
                    <p>Please try refreshing the page</p>
                    ${details ? `<small style="color: #666; margin-top: 10px; display: block;">${details}</small>` : ''}
                `;
                this.container.appendChild(errorElement);

                // Create a simple fallback trophy using Three.js geometry
                this.createFallbackTrophy();
            }

            createFallbackTrophy() {
                // Clear existing scene
                while(this.scene.children.length > 0){ 
                    this.scene.remove(this.scene.children[0]); 
                }

                // Add lights
                const ambientLight = new THREE.AmbientLight(0xffffff, 0.5);
                this.scene.add(ambientLight);

                const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
                directionalLight.position.set(5, 5, 5);
                this.scene.add(directionalLight);

                // Create a simple trophy shape
                const baseGeometry = new THREE.CylinderGeometry(0.8, 1, 0.2, 32);
                const stemGeometry = new THREE.CylinderGeometry(0.2, 0.2, 2, 32);
                const bowlGeometry = new THREE.CylinderGeometry(1, 0.3, 1, 32);

                const material = new THREE.MeshStandardMaterial({
                    color: 0xffd700,
                    metalness: 1,
                    roughness: 0.3,
                });

                const base = new THREE.Mesh(baseGeometry, material);
                const stem = new THREE.Mesh(stemGeometry, material);
                const bowl = new THREE.Mesh(bowlGeometry, material);

                base.position.y = -1.5;
                stem.position.y = 0;
                bowl.position.y = 1.5;

                const trophyGroup = new THREE.Group();
                trophyGroup.add(base);
                trophyGroup.add(stem);
                trophyGroup.add(bowl);

                this.scene.add(trophyGroup);
            }

            loadModel(url) {
                this.loadingElement.style.display = 'block';
                
                // Remove any existing error messages
                const existingErrors = this.container.querySelectorAll('.trophy-error');
                existingErrors.forEach(el => el.remove());
                
                const loader = new THREE.GLTFLoader();
                loader.load(
                    url,
                    (gltf) => {
                        this.loadingElement.style.display = 'none';
                        
                        // Clear existing model if any
                        while(this.scene.children.length > 0){ 
                            this.scene.remove(this.scene.children[0]); 
                        }

                        // Add lights back
                        const ambientLight = new THREE.AmbientLight(0xffffff, 0.5);
                        this.scene.add(ambientLight);

                        const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
                        directionalLight.position.set(5, 5, 5);
                        this.scene.add(directionalLight);

                        // Add new model
                        this.scene.add(gltf.scene);

                        // Center and scale model
                        const box = new THREE.Box3().setFromObject(gltf.scene);
                        const center = box.getCenter(new THREE.Vector3());
                        const size = box.getSize(new THREE.Vector3());

                        const maxDim = Math.max(size.x, size.y, size.z);
                        const scale = 3 / maxDim;
                        gltf.scene.scale.set(scale, scale, scale);

                        gltf.scene.position.x = -center.x * scale;
                        gltf.scene.position.y = -center.y * scale;
                        gltf.scene.position.z = -center.z * scale;

                        // Reset controls
                        this.controls.reset();
                    },
                    (xhr) => {
                        if (xhr.lengthComputable) {
                            const percent = (xhr.loaded / xhr.total * 100).toFixed(0);
                            this.loadingElement.innerHTML = `
                                <div class="loading-spinner"></div>
                                <p>Loading trophy model... ${percent}%</p>
                            `;
                        }
                    },
                    (error) => {
                        console.error('Error loading model:', error);
                        this.loadingElement.style.display = 'none';
                        this.showError(
                            'Failed to load trophy model',
                            `URL: ${url}\nStatus: ${error.target?.status || 'unknown'}\nType: ${error.type || 'unknown'}`
                        );
                    }
                );
            }

            onWindowResize() {
                this.camera.aspect = this.container.clientWidth / this.container.clientHeight;
                this.camera.updateProjectionMatrix();
                this.renderer.setSize(this.container.clientWidth, this.container.clientHeight);
            }

            animate() {
                requestAnimationFrame(() => this.animate());
                this.controls.update();
                this.renderer.render(this.scene, this.camera);
            }
        }

        // Modified showTeamDetail function to use the 3D viewer
        function showTeamDetail(school, sport, year, yearData) {
            const modal = document.getElementById('detailModal');
            const modalBody = modal.querySelector('.modal-body');

            modalBody.innerHTML = `
                <h2>${school.name} ${sport.name} - ${year}</h2>
                <h3>${yearData.achievement}</h3>
                
                <div class="trophy-display">
                    <div id="trophy-container" class="trophy-container"></div>
                    <div class="trophy-controls">
                        <button class="trophy-control-btn" onclick="viewer.controls.reset()">
                            <i class="fas fa-sync-alt"></i>
                        </button>
                    </div>
                </div>

                <div class="team-grid">
                    ${yearData.roster.map(player => `
                        <div class="player-card" onclick="showPlayerStats(${JSON.stringify(player)})">
                            <h4>${player.name}</h4>
                            <p>#${player.number} - ${player.position}</p>
                        </div>
                    `).join('')}
                </div>
            `;

            modal.style.display = 'block';

            // Initialize the trophy viewer after the modal is shown
            setTimeout(() => {
                window.viewer = new TrophyViewer('trophy-container');
                if (yearData.trophyModel) {
                    viewer.loadModel(yearData.trophyModel);
                }
            }, 100);
        }

        function showPlayerStats(player) {
            const modal = document.getElementById('detailModal');
            const modalBody = modal.querySelector('.modal-body');

            modalBody.innerHTML = `
                <h2>${player.name}</h2>
                <h3>#${player.number} - ${player.position}</h3>
                
                <div style="margin-top: 2rem;">
                    <h4>Statistics</h4>
                    ${Object.entries(player.stats).map(([stat, value]) => `
                        <p>${stat.replace(/([A-Z])/g, ' $1').toLowerCase()}: ${value}</p>
                    `).join('')}
                </div>
            `;
        }

        function showAthletesList(athletes) {
            const modal = document.getElementById('detailModal');
            const modalBody = modal.querySelector('.modal-body');

            modalBody.innerHTML = `
                <h2>${athletes[0].college ? 'D1 Athletes' : 'Professional Athletes'}</h2>
                <div class="team-grid">
                    ${athletes.map(athlete => `
                        <div class="player-card">
                            <h4>${athlete.name}</h4>
                            <p>${athlete.sport}</p>
                            <p>${athlete.college || athlete.professional}</p>
                            <p>Class of ${athlete.graduationYear}</p>
                        </div>
                    `).join('')}
                </div>
            `;

            modal.style.display = 'block';
        }

        function updateBreadcrumb() {
            const breadcrumb = document.querySelector('.breadcrumb');
            let path = ['Home'];
            
            navigationHistory.forEach(item => {
                if (item.type === 'school') {
                    const school = currentData.schools.find(s => s.id === item.id);
                    path.push(school.name);
                } else if (item.type === 'sport') {
                    path.push(item.name);
                }
            });

            breadcrumb.innerHTML = path.join(' <span>/</span> ');
        }

        // Close modal when clicking the close button or outside the modal
        document.querySelector('.close-modal').addEventListener('click', () => {
            document.getElementById('detailModal').style.display = 'none';
        });

        window.addEventListener('click', (e) => {
            const modal = document.getElementById('detailModal');
            if (e.target === modal) {
                modal.style.display = 'none';
            }
        });

        // Initialize the application
        loadData();
    </script>
</body>
</html>

```



I have this html app that pulls data from data.json.

We have modified the data to be stored in 3 tables.

Schools, Teams, and Individuals.

Schools has fields, name, primary_color, secondary_color, accent_color, media_url, table

Teams has fields school_name, sport_name, year, achievement, team_photo_url, trophy_model_url, media_url, table

Individuals has fields school_name, team_sport, team_year, type	name, media_url, number, position, graduation_year, sport, college, professional_team, points_per_game, assists, rebounds, passing_yards, touchdowns, completion_percentage, other_stat_name, other_stat_value, table

where table will be the same for every row in the table and just gives the name of the table.

So instead of using the data.json file, we want to use the api to pull the data. 

there aren't any ids, but the names in each table match up to the ones in the other tables.

You can have multiple entries of the same individual signifying multiple teams they were a part of
or multiple entries of the same teams signifying differant years of the same team.

You will need to really think through how to display and organize all the data. there is some new and modified data from the existing index.html and data.json file. 
You will need to strategically think of how the app should work and be organized to show all the data in a way that is easy to understand and navigate and makes sense as a digital trophy case.

The data for each table can be called for separately like this:

Sample post call for Individuals:
```
curl --location 'https://budibase.skoop.digital/api/public/v1/queries/query_datasource_plus_56025c99333e46b8a073e70585159c4c_f62962e3b6ff47e690977d49cd6d6165' \
--header 'x-budibase-app-id: app_3ea495f0892c4311badfd934783afd94' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-budibase-api-key: 69b1bc10c2d32ee607e44e4a30f7f5ea-b9dd09ff6c773a4628f737a0ab45f9e0304e6a8c85181a90f8ff74cc29c946528f373cc83f24ae20' \
--data '{
}'
```

sample response data for Individuals:
```
[
  {
    "data": [
      {
        "rowNumber": 2,
        "_id": 2,
        "school_name": "Saginaw High",
        "team_sport": "Men's Basketball",
        "team_year": "2023",
        "type": "player",
        "name": "John Smith",
        "media_url": "https://saginaw-trophy-media.s3.amazonaws.com/items/individuals-[2].webp",
        "number": "23",
        "position": "Guard",
        "graduation_year": "",
        "sport": "",
        "college": "Basketball",
        "professional_team": "",
        "points_per_game": "",
        "assists": "18.5",
        "rebounds": "5.2",
        "passing_yards": "4.1"
      },
      {
        "rowNumber": 3,
        "_id": 3,
        "school_name": "Saginaw High",
        "team_sport": "Men's Basketball",
        "team_year": "2022",
        "type": "player",
        "name": "John Smith",
        "media_url": "https://saginaw-trophy-media.s3.amazonaws.com/items/individuals-[2].webp",
        "number": "23",
        "position": "Guard",
        "graduation_year": "",
        "sport": "",
        "college": "Basketball",
        "professional_team": "",
        "points_per_game": "",
        "assists": "16.7",
        "rebounds": "4.8",
        "passing_yards": "3.9"
      },
      {
        "rowNumber": 4,
        "_id": 4,
        "school_name": "Saginaw High",
        "team_sport": "Men's Basketball",
        "team_year": "2023",
        "type": "player",
        "name": "Alex Johnson",
        "media_url": "placeholder_url",
        "number": "5",
        "position": "Forward",
        "graduation_year": "",
        "sport": "",
        "college": "Basketball",
        "professional_team": "",
        "points_per_game": "",
        "assists": "12.3",
        "rebounds": "2.5",
        "passing_yards": "7.4"
      },
      {
        "rowNumber": 5,
        "_id": 5,
        "school_name": "Saginaw High",
        "team_sport": "Men's Basketball",
        "team_year": "2022",
        "type": "player",
        "name": "Alex Johnson",
        "media_url": "placeholder_url",
        "number": "5",
        "position": "Forward",
        "graduation_year": "",
        "sport": "",
        "college": "Basketball",
        "professional_team": "",
        "points_per_game": "",
        "assists": "10.8",
        "rebounds": "2.8",
        "passing_yards": "6.9"
      },
      {
        "rowNumber": 6,
        "_id": 6,
        "school_name": "Arthur Hill",
        "team_sport": "Women's Basketball",
        "team_year": "2022",
        "type": "player",
        "name": "Sarah Johnson",
        "media_url": "https://saginaw-trophy-media.s3.amazonaws.com/items/individuals-[6].webp",
        "number": "11",
        "position": "Forward",
        "graduation_year": "",
        "sport": "",
        "college": "Basketball",
        "professional_team": "",
        "points_per_game": "",
        "assists": "15.8",
        "rebounds": "3.2",
        "passing_yards": "8.4"
      },
      {
        "rowNumber": 7,
        "_id": 7,
        "school_name": "Saginaw United",
        "team_sport": "Football",
        "team_year": "2023",
        "type": "player",
        "name": "Marcus Williams",
        "media_url": "https://saginaw-trophy-media.s3.amazonaws.com/items/individuals-[7].webp",
        "number": "7",
        "position": "Quarterback",
        "graduation_year": "",
        "sport": "",
        "college": "Football",
        "professional_team": "",
        "points_per_game": "",
        "assists": "",
        "rebounds": "",
        "passing_yards": "",
        "touchdowns": "2500",
        "completion_percentage": "28",
        "other_stat_name": "65.4"
      },
      {
        "rowNumber": 8,
        "_id": 8,
        "school_name": "Saginaw High",
        "team_sport": "",
        "team_year": "",
        "type": "d1_athlete",
        "name": "Michael Thompson",
        "media_url": "https://saginaw-trophy-media.s3.amazonaws.com/items/individuals-[8].webp",
        "number": "",
        "position": "",
        "graduation_year": "2020",
        "sport": "Basketball",
        "college": "Michigan State"
      },
      {
        "rowNumber": 9,
        "_id": 9,
        "school_name": "Saginaw High",
        "team_sport": "",
        "team_year": "",
        "type": "pro_athlete",
        "name": "LaMarr Woodley",
        "media_url": "https://saginaw-trophy-media.s3.amazonaws.com/items/individuals-[9].webp",
        "number": "",
        "position": "",
        "graduation_year": "2003",
        "sport": "Football",
        "college": "",
        "professional_team": "NFL - Pittsburgh Steelers"
      },
      {
        "rowNumber": 10,
        "_id": 10,
        "school_name": "Saginaw High",
        "team_sport": "Men's Basketball",
        "team_year": "2023",
        "type": "player",
        "name": "John Doe",
        "media_url": "https://saginaw-trophy-media.s3.amazonaws.com/items/individuals-[10].webp",
        "number": "30",
        "position": "Guard",
        "graduation_year": "",
        "sport": "",
        "college": "Basketball",
        "professional_team": "",
        "points_per_game": "",
        "assists": "14.2",
        "rebounds": "6",
        "passing_yards": "5.1"
      },
      {
        "rowNumber": 11,
        "_id": 11,
        "school_name": "Saginaw High",
        "team_sport": "Baseball",
        "team_year": "2023",
        "type": "player",
        "name": "John Doe",
        "media_url": "https://saginaw-trophy-media.s3.amazonaws.com/items/individuals-[11].webp",
        "number": "30",
        "position": "Pitcher",
        "graduation_year": "",
        "sport": "",
        "college": "Baseball"
      }
    ],
    "extra": {}
  }
]
```

Sample post call for Teams:
```
curl --location 'https://budibase.skoop.digital/api/public/v1/queries/query_datasource_plus_56025c99333e46b8a073e70585159c4c_a47c1c9912c34d15b67e49ec62908fa0' \
--header 'x-budibase-app-id: app_3ea495f0892c4311badfd934783afd94' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-budibase-api-key: 69b1bc10c2d32ee607e44e4a30f7f5ea-b9dd09ff6c773a4628f737a0ab45f9e0304e6a8c85181a90f8ff74cc29c946528f373cc83f24ae20' \
--data '{
}'
```

sample response data for Teams:
```
[
  {
    "data": [
      {
        "rowNumber": 2,
        "_id": 2,
        "school_name": "Saginaw High",
        "sport_name": "Men's Basketball",
        "year": "2023",
        "achievement": "District Champions",
        "team_photo_url": "https://saginaw-trophy-media.s3.amazonaws.com/items/team-media-[2].webp",
        "trophy_model_url": "https://saginaw-trophy-media.s3.amazonaws.com/trophy-[2].gltf",
        "media_url": "https://saginaw-trophy-media.s3.amazonaws.com/items/teams-[3].webp"
      },
      {
        "rowNumber": 3,
        "_id": 3,
        "school_name": "Arthur Hill",
        "sport_name": "Women's Basketball",
        "year": "2022",
        "achievement": "District Champions",
        "team_photo_url": "https://saginaw-trophy-media.s3.amazonaws.com/items/team-media-[3].webp",
        "trophy_model_url": "https://saginaw-trophy-media.s3.amazonaws.com/trophy-[3].gltf",
        "media_url": "https://saginaw-trophy-media.s3.amazonaws.com/items/teams-[3].webp"
      },
      {
        "rowNumber": 4,
        "_id": 4,
        "school_name": "Saginaw United",
        "sport_name": "Football",
        "year": "2023",
        "achievement": "District Champions",
        "team_photo_url": "https://saginaw-trophy-media.s3.amazonaws.com/items/team-media-[4].webp",
        "trophy_model_url": "https://saginaw-trophy-media.s3.amazonaws.com/trophy-[2].gltf",
        "media_url": "https://saginaw-trophy-media.s3.amazonaws.com/items/teams-[4].webp"
      }
    ],
    "extra": {}
  }
]
```


Sample post call for Schools:
```
curl --location 'https://budibase.skoop.digital/api/public/v1/queries/query_datasource_plus_56025c99333e46b8a073e70585159c4c_4a3b9f6c024c4381afcf9f4e83f85f7d' \
--header 'x-budibase-app-id: app_3ea495f0892c4311badfd934783afd94' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-budibase-api-key: 69b1bc10c2d32ee607e44e4a30f7f5ea-b9dd09ff6c773a4628f737a0ab45f9e0304e6a8c85181a90f8ff74cc29c946528f373cc83f24ae20' \
--data '{
}'
```

sample response data for Schools:
```
[
  {
    "data": [
      {
        "rowNumber": 2,
        "_id": 2,
        "name": "Saginaw High",
        "primary_color": "#000000",
        "secondary_color": "#EFBF04",
        "accent_color": "#757575",
        "media_url": "https://saginaw-trophy-media.s3.amazonaws.com/items/schools-[2].webp"
      },
      {
        "rowNumber": 3,
        "_id": 3,
        "name": "Arthur Hill",
        "primary_color": "#000080",
        "secondary_color": "#EFBF04",
        "accent_color": "#757575",
        "media_url": "https://saginaw-trophy-media.s3.amazonaws.com/items/schools-[3].webp"
      },
      {
        "rowNumber": 4,
        "_id": 4,
        "name": "Saginaw United",
        "primary_color": "#000000",
        "secondary_color": "#EFBF04",
        "accent_color": "#757575",
        "media_url": "https://saginaw-trophy-media.s3.amazonaws.com/items/schools-[4].webp"
      }
    ],
    "extra": {}
  }
]
```

you must workaround cors issues to get the data from the api so that you dont get errors like this:
```
lockdown-install.js:1 Removing unpermitted intrinsics
trophy-demo/:1  Access to fetch at 'https://budibase.skoop.digital/api/public/v1/queries/query_datasource_plus_56025c99333e46b8a073e70585159c4c_f62962e3b6ff47e690977d49cd6d6165' from origin 'https://skoop-digital.github.io' has been blocked by CORS policy: Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource. If an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.
trophy-demo/:515 

            POST https://budibase.skoop.digital/api/public/v1/queries/query_datasource_plus_56025c99333e46b8a073e70585159c4c_f62962e3b6ff47e690977d49cd6d6165 net::ERR_FAILED
fetchData @ trophy-demo/:515
loadAllData @ trophy-demo/:543
trophy-demo/:528  Error fetching data from query_datasource_plus_56025c99333e46b8a073e70585159c4c_f62962e3b6ff47e690977d49cd6d6165: TypeError: Failed to fetch
    at fetchData (trophy-demo/:515:40)
    at HTMLDocument.loadAllData (trophy-demo/:543:21)
fetchData @ trophy-demo/:528
await in fetchData
loadAllData @ trophy-demo/:543
[NEW] Explain Console errors by using Copilot in Edge: click

         to explain an error. 
        Learn more
        Don't show again
trophy-demo/:1  Access to fetch at 'https://budibase.skoop.digital/api/public/v1/queries/query_datasource_plus_56025c99333e46b8a073e70585159c4c_4a3b9f6c024c4381afcf9f4e83f85f7d' from origin 'https://skoop-digital.github.io' has been blocked by CORS policy: Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource. If an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.
trophy-demo/:515 

            POST https://budibase.skoop.digital/api/public/v1/queries/query_datasource_plus_56025c99333e46b8a073e70585159c4c_4a3b9f6c024c4381afcf9f4e83f85f7d net::ERR_FAILED
fetchData @ trophy-demo/:515
loadAllData @ trophy-demo/:541
trophy-demo/:528  Error fetching data from query_datasource_plus_56025c99333e46b8a073e70585159c4c_4a3b9f6c024c4381afcf9f4e83f85f7d: TypeError: Failed to fetch
    at fetchData (trophy-demo/:515:40)
    at HTMLDocument.loadAllData (trophy-demo/:541:21)
fetchData @ trophy-demo/:528
await in fetchData
loadAllData @ trophy-demo/:541
trophy-demo/:1  Access to fetch at 'https://budibase.skoop.digital/api/public/v1/queries/query_datasource_plus_56025c99333e46b8a073e70585159c4c_a47c1c9912c34d15b67e49ec62908fa0' from origin 'https://skoop-digital.github.io' has been blocked by CORS policy: Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource. If an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.
trophy-demo/:515 

            POST https://budibase.skoop.digital/api/public/v1/queries/query_datasource_plus_56025c99333e46b8a073e70585159c4c_a47c1c9912c34d15b67e49ec62908fa0 net::ERR_FAILED
fetchData @ trophy-demo/:515
loadAllData @ trophy-demo/:542
trophy-demo/:528  Error fetching data from query_datasource_plus_56025c99333e46b8a073e70585159c4c_a47c1c9912c34d15b67e49ec62908fa0: TypeError: Failed to fetch
    at fetchData (trophy-demo/:515:40)
    at HTMLDocument.loadAllData (trophy-demo/:542:21)
```

Do not use mock data, we must use the api calls to get the data.

please re-write the whole index.html file to use the api calls to get the data instead of the data.json file. 
Make sure to keep everything in the 1 htrml file, using inline styles or style/script tags and imports.

use an artifact in case we need to modify it later.