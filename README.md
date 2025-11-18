<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ParkLocator - Find & Track Parking</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background-color: #f5f7fa;
            color: #333;
            line-height: 1.6;
        }
        
        header {
            background: linear-gradient(135deg, #2c3e50, #4a6491);
            color: white;
            padding: 1.5rem;
            text-align: center;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        }
        
        header h1 {
            font-size: 2.2rem;
            margin-bottom: 0.5rem;
        }
        
        header p {
            font-size: 1.1rem;
            opacity: 0.9;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 2rem;
        }
        
        .app-description {
            background-color: white;
            border-radius: 10px;
            padding: 2rem;
            margin-bottom: 2rem;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.05);
        }
        
        .app-description h2 {
            color: #2c3e50;
            margin-bottom: 1rem;
            font-size: 1.8rem;
        }
        
        .app-description p {
            margin-bottom: 1rem;
            font-size: 1.1rem;
            color: #555;
        }
        
        .features {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 1.5rem;
            margin-bottom: 2rem;
        }
        
        .feature-card {
            background-color: white;
            border-radius: 10px;
            padding: 1.5rem;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.05);
            transition: transform 0.3s ease;
        }
        
        .feature-card:hover {
            transform: translateY(-5px);
        }
        
        .feature-card h3 {
            color: #2c3e50;
            margin-bottom: 1rem;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }
        
        .feature-card p {
            color: #666;
        }
        
        .demo-section {
            background-color: white;
            border-radius: 10px;
            padding: 2rem;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.05);
            margin-bottom: 2rem;
        }
        
        .demo-section h2 {
            color: #2c3e50;
            margin-bottom: 1.5rem;
            text-align: center;
            font-size: 1.8rem;
        }
        
        .map-container {
            height: 500px;
            border-radius: 10px;
            overflow: hidden;
            margin-bottom: 1.5rem;
            border: 1px solid #e0e0e0;
            position: relative;
        }
        
        #map {
            height: 100%;
            width: 100%;
            background-color: #e9f2fa;
            position: relative;
        }
        
        .map-overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            background-color: rgba(255, 255, 255, 0.9);
            z-index: 10;
            text-align: center;
            padding: 2rem;
        }
        
        .map-overlay.hidden {
            display: none;
        }
        
        .map-overlay h3 {
            margin-bottom: 1rem;
            color: #2c3e50;
        }
        
        .map-overlay p {
            margin-bottom: 1.5rem;
            max-width: 500px;
            color: #555;
        }
        
        .map-grid {
            display: grid;
            grid-template-columns: repeat(20, 1fr);
            grid-template-rows: repeat(20, 1fr);
            width: 100%;
            height: 100%;
            position: relative;
        }
        
        .grid-cell {
            border: 1px solid rgba(0, 0, 0, 0.05);
        }
        
        .parking-spot {
            position: absolute;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            transform: translate(-50%, -50%);
            border: 2px solid white;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
        }
        
        .parking-available {
            background-color: #2ecc71;
        }
        
        .parking-occupied {
            background-color: #e74c3c;
        }
        
        .user-location {
            position: absolute;
            width: 24px;
            height: 24px;
            border-radius: 50%;
            background-color: #3498db;
            border: 3px solid white;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.3);
            transform: translate(-50%, -50%);
            z-index: 5;
        }
        
        .car-location {
            position: absolute;
            width: 30px;
            height: 30px;
            border-radius: 50%;
            background-color: #e74c3c;
            border: 3px solid white;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.3);
            transform: translate(-50%, -50%);
            z-index: 5;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 14px;
        }
        
        .navigation-line {
            position: absolute;
            height: 4px;
            background-color: #3498db;
            transform-origin: 0 0;
            z-index: 4;
        }
        
        .controls {
            display: flex;
            justify-content: center;
            gap: 1rem;
            flex-wrap: wrap;
            margin-top: 1.5rem;
        }
        
        button {
            padding: 0.8rem 1.5rem;
            background: linear-gradient(135deg, #3498db, #2c3e50);
            color: white;
            border: none;
            border-radius: 50px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            gap: 0.5rem;
            box-shadow: 0 4px 10px rgba(52, 152, 219, 0.3);
        }
        
        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 15px rgba(52, 152, 219, 0.4);
        }
        
        button:active {
            transform: translateY(0);
        }
        
        button:disabled {
            background: #95a5a6;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }
        
        .status-message {
            text-align: center;
            margin-top: 1rem;
            padding: 1rem;
            border-radius: 8px;
            font-weight: 500;
        }
        
        .success {
            background-color: rgba(46, 204, 113, 0.2);
            color: #27ae60;
        }
        
        .warning {
            background-color: rgba(241, 196, 15, 0.2);
            color: #f39c12;
        }
        
        .info {
            background-color: rgba(52, 152, 219, 0.2);
            color: #2980b9;
        }
        
        footer {
            background-color: #2c3e50;
            color: white;
            text-align: center;
            padding: 2rem;
            margin-top: 2rem;
        }
        
        footer p {
            margin-bottom: 0.5rem;
        }
        
        @media (max-width: 768px) {
            .container {
                padding: 1rem;
            }
            
            .map-container {
                height: 400px;
            }
            
            header h1 {
                font-size: 1.8rem;
            }
        }
        
        .icon {
            font-size: 1.2rem;
        }
    </style>
</head>
<body>
    <header>
        <h1>ParkLocator</h1>
        <p>Find parking spots and never forget where you parked again</p>
    </header>
    
    <div class="container">
        <section class="app-description">
            <h2>Your Parking Solution</h2>
            <p>ParkLocator helps you find available parking spots in real-time and automatically saves your parking location so you can easily find your way back to your vehicle.</p>
            <p>Using location technology, our app pinpoints your exact parking location and provides directions when you're ready to return to your car.</p>
        </section>
        
        <section class="features">
            <div class="feature-card">
                <h3><span class="icon">📍</span> Real-Time Parking Availability</h3>
                <p>See available parking spots in your area with updates to save time searching for parking.</p>
            </div>
            <div class="feature-card">
                <h3><span class="icon">🚗</span> Auto Parking Location Save</h3>
                <p>Automatically save your parking location with a single tap. No more forgetting where you parked.</p>
            </div>
            <div class="feature-card">
                <h3><span class="icon">🗺️</span> Turn-by-Turn Navigation</h3>
                <p>Get directions back to your parked car with integrated navigation.</p>
            </div>
        </section>
        
        <section class="demo-section">
            <h2>Try ParkLocator Demo</h2>
            <div class="map-container">
                <div id="map">
                    <div class="map-overlay" id="mapOverlay">
                        <h3>Parking Map</h3>
                        <p>Click "Find Parking Nearby" to start using ParkLocator and see available parking spots in your area.</p>
                        <button id="startDemo">
                            <span class="icon">🚀</span> Start Demo
                        </button>
                    </div>
                    <div class="map-grid" id="mapGrid"></div>
                </div>
            </div>
            <div class="controls">
                <button id="findParking">
                    <span class="icon">🔍</span> Find Parking Nearby
                </button>
                <button id="saveLocation" disabled>
                    <span class="icon">💾</span> Save My Parking Location
                </button>
                <button id="navigateToCar" disabled>
                    <span class="icon">🧭</span> Navigate to My Car
                </button>
            </div>
            <div id="statusMessage" class="status-message info">
                Welcome to ParkLocator! Click "Find Parking Nearby" to see available spots.
            </div>
        </section>
    </div>
    
    <footer>
        <p>© 2023 ParkLocator - Never Lose Your Parking Spot Again</p>
        <p>Contact: info@parklocator.com | Privacy Policy | Terms of Service</p>
    </footer>

    <script>
        // DOM Elements
        const mapOverlay = document.getElementById('mapOverlay');
        const mapGrid = document.getElementById('mapGrid');
        const findParkingBtn = document.getElementById('findParking');
        const saveLocationBtn = document.getElementById('saveLocation');
        const navigateToCarBtn = document.getElementById('navigateToCar');
        const statusMessage = document.getElementById('statusMessage');
        const startDemoBtn = document.getElementById('startDemo');
        
        // Application state
        let userLocation = null;
        let carLocation = null;
        let parkingSpots = [];
        let navigationLine = null;
        
        // Initialize the application
        function initApp() {
            createMapGrid();
            generateParkingSpots();
            
            // Event listeners
            findParkingBtn.addEventListener('click', findUserLocation);
            saveLocationBtn.addEventListener('click', saveParkingLocation);
            navigateToCarBtn.addEventListener('click', navigateToCar);
            startDemoBtn.addEventListener('click', startDemo);
        }
        
        // Create the grid for the map
        function createMapGrid() {
            mapGrid.innerHTML = '';
            for (let i = 0; i < 400; i++) {
                const cell = document.createElement('div');
                cell.className = 'grid-cell';
                mapGrid.appendChild(cell);
            }
        }
        
        // Generate random parking spots
        function generateParkingSpots() {
            parkingSpots = [];
            
            // Create 15 random parking spots
            for (let i = 0; i < 15; i++) {
                const available = Math.random() > 0.4; // 60% chance of being available
                const top = Math.floor(Math.random() * 90) + 5; // 5% to 95%
                const left = Math.floor(Math.random() * 90) + 5; // 5% to 95%
                
                parkingSpots.push({
                    id: i,
                    available,
                    top,
                    left
                });
            }
            
            renderParkingSpots();
        }
        
        // Render parking spots on the map
        function renderParkingSpots() {
            // Remove existing parking spots
            document.querySelectorAll('.parking-spot').forEach(spot => spot.remove());
            
            // Add parking spots to the map
            parkingSpots.forEach(spot => {
                const parkingElement = document.createElement('div');
                parkingElement.className = `parking-spot ${spot.available ? 'parking-available' : 'parking-occupied'}`;
                parkingElement.style.top = `${spot.top}%`;
                parkingElement.style.left = `${spot.left}%`;
                parkingElement.setAttribute('data-id', spot.id);
                
                parkingElement.addEventListener('click', () => {
                    const status = spot.available ? 'Available' : 'Occupied';
                    statusMessage.textContent = `Parking spot ${status}. ${spot.available ? 'You can park here!' : 'This spot is taken.'}`;
                    statusMessage.className = `status-message ${spot.available ? 'success' : 'warning'}`;
                });
                
                mapGrid.appendChild(parkingElement);
            });
        }
        
        // Start the demo
        function startDemo() {
            mapOverlay.classList.add('hidden');
            statusMessage.textContent = 'Demo started! Click "Find Parking Nearby" to locate available parking spots.';
            statusMessage.className = 'status-message info';
            findParkingBtn.disabled = false;
        }
        
        // Find user's location (simulated)
        function findUserLocation() {
            // In a real app, this would use navigator.geolocation
            // For demo purposes, we'll simulate a location
            
            statusMessage.textContent = 'Finding your location...';
            statusMessage.className = 'status-message info';
            
            // Simulate API call delay
            setTimeout(() => {
                // Remove existing user location
                document.querySelectorAll('.user-location').forEach(el => el.remove());
                
                // Set a random user location (not on top of a parking spot)
                let userTop, userLeft;
                let validLocation = false;
                
                while (!validLocation) {
                    userTop = Math.floor(Math.random() * 80) + 10; // 10% to 90%
                    userLeft = Math.floor(Math.random() * 80) + 10; // 10% to 90%
                    
                    // Check if too close to any parking spot
                    validLocation = !parkingSpots.some(spot => {
                        const distance = Math.sqrt(
                            Math.pow(spot.top - userTop, 2) + Math.pow(spot.left - userLeft, 2)
                        );
                        return distance < 8; // Minimum distance from parking spots
                    });
                }
                
                userLocation = { top: userTop, left: userLeft };
                
                // Add user location to the map
                const userElement = document.createElement('div');
                userElement.className = 'user-location';
                userElement.style.top = `${userTop}%`;
                userElement.style.left = `${userLeft}%`;
                mapGrid.appendChild(userElement);
                
                statusMessage.textContent = 'Location found! Available parking spots are shown in green.';
                statusMessage.className = 'status-message success';
                
                // Enable save location button
                saveLocationBtn.disabled = false;
            }, 1500);
        }
        
        // Save parking location
        function saveParkingLocation() {
            if (!userLocation) {
                statusMessage.textContent = 'Please find your location first.';
                statusMessage.className = 'status-message warning';
                return;
            }
            
            // Remove existing car location
            document.querySelectorAll('.car-location').forEach(el => el.remove());
            if (navigationLine) {
                navigationLine.remove();
                navigationLine = null;
            }
            
            // Set car location at user's current position
            carLocation = { ...userLocation };
            
            // Add car location to the map
            const carElement = document.createElement('div');
            carElement.className = 'car-location';
            carElement.innerHTML = '🚗';
            carElement.style.top = `${carLocation.top}%`;
            carElement.style.left = `${carLocation.left}%`;
            mapGrid.appendChild(carElement);
            
            statusMessage.textContent = 'Parking location saved! You can now navigate back to your car.';
            statusMessage.className = 'status-message success';
            
            // Enable navigate to car button
            navigateToCarBtn.disabled = false;
        }
        
        // Navigate to car
        function navigateToCar() {
            if (!carLocation) {
                statusMessage.textContent = 'No car location saved. Please save your parking location first.';
                statusMessage.className = 'status-message warning';
                return;
            }
            
            if (!userLocation) {
                statusMessage.textContent = 'Please find your location first.';
                statusMessage.className = 'status-message warning';
                return;
            }
            
            // Remove existing navigation line
            if (navigationLine) {
                navigationLine.remove();
            }
            
            // Calculate distance and angle for the navigation line
            const dx = carLocation.left - userLocation.left;
            const dy = carLocation.top - userLocation.top;
            const distance = Math.sqrt(dx * dx + dy * dy);
            const angle = Math.atan2(dy, dx) * (180 / Math.PI);
            
            // Create navigation line
            navigationLine = document.createElement('div');
            navigationLine.className = 'navigation-line';
            navigationLine.style.width = `${distance}%`;
            navigationLine.style.top = `${userLocation.top}%`;
            navigationLine.style.left = `${userLocation.left}%`;
            navigationLine.style.transform = `rotate(${angle}deg)`;
            mapGrid.appendChild(navigationLine);
            
            statusMessage.textContent = 'Navigation activated! Follow the blue line to your car.';
            statusMessage.className = 'status-message info';
            
            // Simulate user moving towards the car
            simulateUserMovement();
        }
        
        // Simulate user moving towards the car
        function simulateUserMovement() {
            if (!userLocation || !carLocation) return;
            
            const steps = 50;
            let currentStep = 0;
            
            const moveInterval = setInterval(() => {
                if (currentStep >= steps) {
                    clearInterval(moveInterval);
                    statusMessage.textContent = 'You have reached your car!';
                    statusMessage.className = 'status-message success';
                    return;
                }
                
                currentStep++;
                
                // Calculate new position
                const progress = currentStep / steps;
                const newTop = userLocation.top + (carLocation.top - userLocation.top) * progress;
                const newLeft = userLocation.left + (carLocation.left - userLocation.left) * progress;
                
                // Update user location
                const userElement = document.querySelector('.user-location');
                if (userElement) {
                    userElement.style.top = `${newTop}%`;
                    userElement.style.left = `${newLeft}%`;
                }
                
                // Update navigation line
                if (navigationLine) {
                    const dx = carLocation.left - newLeft;
                    const dy = carLocation.top - newTop;
                    const distance = Math.sqrt(dx * dx + dy * dy);
                    const angle = Math.atan2(dy, dx) * (180 / Math.PI);
                    
                    navigationLine.style.width = `${distance}%`;
                    navigationLine.style.top = `${newTop}%`;
                    navigationLine.style.left = `${newLeft}%`;
                    navigationLine.style.transform = `rotate(${angle}deg)`;
                }
            }, 100);
        }
        
        // Initialize the app when the page loads
        document.addEventListener('DOMContentLoaded', initApp);
    </script>
</body>
</html>
