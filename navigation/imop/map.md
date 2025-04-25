---
layout: page
title: IMOP San Diego Map
search_exclude: true
permalink: /map
menu: nav/imop.html
---

<div class="form-container">
    <h2>Map of San Diego Locations</h2>
    <form id="selectionForm">
        <label for="location">Enter City</label>
        <input id="place" name="place" type="text" placeholder="San Diego" />
        <button id="goButton">Go</button>
    </form>
</div>

<!-- Map Section -->
<div id="map" style="height: 400px; margin-top: 20px; border-radius: 10px;"></div>

<!-- Locations List Section -->
<div class="locations-container">
    <h3>Locations Data</h3>
    <table id="locationsTable">
        <thead>
            <tr>
                <th>Building Name</th>
                <th>Latitude</th>
                <th>Longitude</th>
                <th>Condition</th>
            </tr>
        </thead>
        <tbody>
            <!-- Dynamically populated rows will go here -->
        </tbody>
    </table>
</div>

<div class="schedule-container">
    <h3>Maintenance Schedule</h3>
    <table id="scheduleTable">
        <thead>
            <tr>
                <th>Date</th>
                <th>Time</th>
                <th>Task</th>
                <th>Description</th>
                <th>Actions</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>April 10, 2025</td>
                <td>9:00 AM</td>
                <td>Pressure Wash</td>
                <td>Clean the exterior walls of the building.</td>
                <td><button class="button" onclick="viewTaskDetails('Pressure Wash', 'Clean the exterior walls of the building')">View</button></td>
            </tr>
            <tr>
                <td>April 12, 2025</td>
                <td>2:00 PM</td>
                <td>HVAC Check</td>
                <td>Inspect and service the HVAC system for maintenance.</td>
                <td><button class="button" onclick="viewTaskDetails('HVAC Check', 'Inspect and service the HVAC system for maintenance')">View</button></td>
            </tr>
            <tr>
                <td>April 15, 2025</td>
                <td>10:30 AM</td>
                <td>Window Cleaning</td>
                <td>Clean all the windows inside and outside.</td>
                <td><button class="button" onclick="viewTaskDetails('Window Cleaning', 'Clean all the windows inside and outside')">View</button></td>
            </tr>
            <tr>
                <td>April 20, 2025</td>
                <td>8:00 AM</td>
                <td>Electrical Check</td>
                <td>Inspect and test all electrical systems and connections.</td>
                <td><button class="button" onclick="viewTaskDetails('Electrical Check', 'Inspect and test all electrical systems and connections')">View</button></td>
            </tr>
        </tbody>
    </table>
</div>

<!-- Footer Section -->
<footer class="footer">
    <p>&copy; 2024 Waypoints. All Rights Reserved.</p>
</footer>

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script type="module">
    import { pythonURI } from "{{site.baseurl}}/assets/js/api/config.js";

    // Leaflet expects this shadow image to be available
    L.Icon.Default.mergeOptions({
        shadowUrl: 'https://unpkg.com/leaflet@1.9.4/dist/images/marker-shadow.png'
    });

    // Initialize the map centered on San Diego
    let map;
    document.addEventListener("DOMContentLoaded", () => {
        map = L.map("map").setView([32.7157, -117.1611], 12); // San Diego coordinates
        L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
            maxZoom: 18,
            attribution: "© OpenStreetMap contributors",
        }).addTo(map);

        // Fetch coordinates from the backend and plot them on the map
        fetchCoordinatesAndUpdate();
    });

    // Function to fetch coordinates and update the map and table
    async function fetchCoordinatesAndUpdate() {
        try {
            const response = await fetch(`${pythonURI}/api/coords`, {
                method: 'GET',
                credentials: 'include', // Include credentials for CORS
                headers: {
                    'Content-Type': 'application/json',
                },
            });

            if (!response.ok) {
                throw new Error(`Failed to fetch coordinates: ${response.status}`);
            }

            const data = await response.json();

            const tableBody = document.querySelector('#locationsTable tbody');
            tableBody.innerHTML = ''; // Clear existing rows

            data.forEach(location => {
                const { lat, lng, building_name, condition } = location;

                // Add data to the locations table
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${building_name}</td>
                    <td>${lat}</td>
                    <td>${lng}</td>
                    <td>${condition}</td>
                `;
                tableBody.appendChild(row);

                // Determine the color based on condition
                let color;
                if (condition === 'Good' || condition === 'green') {
                    color = 'green';
                } else if (condition === 'Moderate' || condition === 'yellow' || condition === 'Fair') {
                    color = 'yellow';
                } else {
                    color = 'red';
                }

                // Create a colored pin icon
                const icon = L.icon({
                    iconUrl: getIconUrl(color),
                    iconSize: [25, 41],
                    iconAnchor: [12, 41],
                    popupAnchor: [0, -41],
                });

                // Place marker on the map
                L.marker([parseFloat(lat), parseFloat(lng)], { icon: icon })
                    .addTo(map)
                    .bindPopup(`<b>${building_name}</b><br>Condition: ${condition}`);
            });
        } catch (error) {
            console.error('Error fetching coordinates:', error);
            alert('Failed to load location data. Please try again later.');
        }
    }

    // Helper to return marker icon URL based on condition color
    function getIconUrl(color) {
        switch (color) {
            case 'green':
                return 'https://raw.githubusercontent.com/pointhi/leaflet-color-markers/master/img/marker-icon-2x-green.png';
            case 'yellow':
                return 'https://raw.githubusercontent.com/pointhi/leaflet-color-markers/master/img/marker-icon-2x-yellow.png';
            case 'red':
                return 'https://raw.githubusercontent.com/pointhi/leaflet-color-markers/master/img/marker-icon-2x-red.png';
            default:
                return 'https://unpkg.com/leaflet@1.9.4/dist/images/marker-icon.png';
        }
    }

    // Handle the "Go" button click event
    const goButton = document.getElementById("goButton");
    goButton.addEventListener("click", function(event) {
        event.preventDefault();
        const place = document.getElementById("place").value.trim();

        if (place.toLowerCase() === "san diego") {
            map.setView([32.7157, -117.1611], 12);
        } else {
            alert("Currently, only San Diego locations are available.");
        }
    });

    // Task detail popup
    function viewTaskDetails(task, description) {
        alert(`Task: ${task}\nDescription: ${description}`);
    }
</script>

```