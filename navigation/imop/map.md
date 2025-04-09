---
layout: tailwinds
title: San Diego Waypoints
search_exclude: true
permalink: /imop/map
---

<head>
  <link rel="stylesheet" href="../../assets/css/travel/waypoints.css" />
  <style>
    /* Styling for the calendar and schedule */
    .schedule-container {
        margin-top: 20px;
        padding: 20px;
        background-color:rgb(60, 60, 62);
        border-radius: 8px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    }
    .schedule-container h3 {
        font-size: 24px;
        margin-bottom: 10px;
    }
    #scheduleTable {
        width: 100%;
        border-collapse: collapse;
        margin-top: 20px;
    }
    #scheduleTable th, #scheduleTable td {
        padding: 12px;
        text-align: center;
        border: 1px solid #ddd;
    }
    #scheduleTable th {
        background-color:rgb(77, 76, 76);
    }
    #scheduleTable td {
        background-color:rgb(145, 142, 142);
    }
    .button {
        padding: 8px 16px;
        background-color: #4CAF50;
        color: white;
        border: none;
        border-radius: 4px;
        cursor: pointer;
    }
    .button:hover {
        background-color: #45a049;
    }
  </style>
</head>

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

<script>
    // Sample data for locations with descriptions in San Diego
    const locations = [
        { lat: 32.7157, lon: -117.1611, name: "San Diego City Center", description: "The vibrant downtown area of San Diego." },
        { lat: 32.7315, lon: -117.1473, name: "Balboa Park", description: "A large urban park with museums and gardens." },
        { lat: 32.7538, lon: -117.2515, name: "Mission Beach", description: "A popular beach area with a boardwalk." },
        { lat: 32.7756, lon: -117.1184, name: "Old Town San Diego", description: "A historic area with museums, shops, and restaurants." },
        { lat: 32.7637, lon: -117.2050, name: "Cabrillo National Monument", description: "A monument offering breathtaking views of the Pacific Ocean." }
    ];

    // Initialize the map centered on San Diego
    let map;
    document.addEventListener("DOMContentLoaded", () => {
        map = L.map("map").setView([32.7157, -117.1611], 12); // San Diego coordinates
        L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
            maxZoom: 18,
            attribution: "© OpenStreetMap contributors",
        }).addTo(map);

        // Add the location markers
        locations.forEach(location => {
            const marker = L.marker([location.lat, location.lon]).addTo(map);
            marker.bindPopup(`<b>${location.name}</b><br>${location.description}`);
        });
    });

    // Handle the "Go" button click event
    const goButton = document.getElementById("goButton");
    goButton.addEventListener("click", function(event) {
        event.preventDefault();
        const place = document.getElementById("place").value.trim();
        
        if (place.toLowerCase() === "san diego") {
            // Reset map view if user searches for "San Diego"
            map.setView([32.7157, -117.1611], 12);
        } else {
            alert("Currently, only San Diego locations are available.");
        }
    });
    function viewTaskDetails(task, description) {
        alert(`Task: ${task}\nDescription: ${description}`);
    }
</script>

