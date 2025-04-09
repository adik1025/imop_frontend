---
layout: post
title: Paris Waypoints
search_exclude: true
permalink: /travel/paris/wellness_waypoints
## menu: nav/paris_hotbar.html
---

<head>
  <link rel="stylesheet" href="../../assets/css/travel/waypoints.css" />
</head>

<div class="form-container">
    <h2>Map Injury to Care Location</h2>
    <form id="selectionForm">
        <label for="injury">Select Injury</label>
        <select id="injury" name="injury" required>
            <option value="">Select Injury</option>
        </select>
        <label for="location">Care Location</label>
        <input id="location" name="location" type="text" disabled />
        <label for="place">Enter City</label>
        <input id="place" name="place" type="text" />
        <button id="goButton">Go</button>
    </form>
</div>
<div id="search-results">
    <table id="resultsTable" class="compact-table">
        <thead>
            <tr>
                <th>#</th>
                <th>Place Name</th>
                <th>Address</th>
                <th>Actions</th>
            </tr>
        </thead>
        <tbody>
        </tbody>
    </table>
</div>
<div id="carecenter-results">
    <table id="carecenterTable" class="compact-table">
        <thead>
            <tr>
                <th>#</th>
                <th>Injury</th>
                <th>Location</th>
                <th>Address</th>
                <th>Rating</th>
                <th>Actions</th>
            </tr>
        </thead>
        <tbody>
        </tbody>
    </table>
</div>
<!-- Map Section -->
<div id="map" style="height: 400px; margin-top: 20px; border-radius: 10px;"></div>

<!-- Footer Section -->
<footer class="footer">
    <p>&copy; 2024 Wellness Waypoints. All Rights Reserved.</p>
</footer>

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

  <script>
      // JavaScript collection of injury and location data
      const data = [
          { _injury: "Fractures", _location: "Hospital" },
          { _injury: "Broken Bones", _location: "Hospital" },
          { _injury: "Severe Bleeding", _location: "Hospital" },
          { _injury: "Head Injuries", _location: "Hospital" },
          { _injury: "Concussions", _location: "Hospital" },
          { _injury: "Heart Attack", _location: "Hospital" },
          { _injury: "Stroke", _location: "Hospital" },
          { _injury: "Appendicitis", _location: "Hospital" },
          { _injury: "Dehydration", _location: "Hospital" },
          { _injury: "Heatstroke", _location: "Hospital" },
          { _injury: "Allergic Reaction", _location: "Hospital" },
          { _injury: "Burns", _location: "Hospital" },
          { _injury: "Respiratory Issues", _location: "Hospital" },
          { _injury: "Infections", _location: "Hospital" },
          { _injury: "Snake Bite", _location: "Hospital" },
          { _injury: "Animal Bite", _location: "Hospital" },
          { _injury: "Minor Cuts", _location: "Pharmacy" },
          { _injury: "Motion Sickness", _location: "Pharmacy" },
          { _injury: "Mild Allergies", _location: "Pharmacy" },
          { _injury: "Upset Stomach", _location: "Pharmacy" },
          { _injury: "Diarrhea", _location: "Pharmacy" },
          { _injury: "Pain", _location: "Pharmacy" },
          { _injury: "Headaches", _location: "Pharmacy" },
          { _injury: "Coughs", _location: "Pharmacy" },
          { _injury: "Colds", _location: "Pharmacy" },
          { _injury: "Insect Bites", _location: "Pharmacy" },
          { _injury: "Stings", _location: "Pharmacy" },
          { _injury: "Sunburn", _location: "Pharmacy" },
          { _injury: "Blisters", _location: "Pharmacy" },
          { _injury: "Skin Irritation", _location: "Pharmacy" },
          { _injury: "Menstrual Pain", _location: "Pharmacy" },
          { _injury: "Muscle Strains", _location: "Recovery" },
          { _injury: "Sprains", _location: "Recovery" },
          { _injury: "Back Pain", _location: "Recovery" },
          { _injury: "Neck Pain", _location: "Recovery" },
          { _injury: "Post-Surgery Recovery", _location: "Recovery" },
          { _injury: "Joint Injuries", _location: "Recovery" },
          { _injury: "Exhaustion", _location: "Recovery" },
          { _injury: "Chronic Fatigue", _location: "Recovery" },
          { _injury: "Mental Health", _location: "Recovery" },
          { _injury: "Substance Overuse", _location: "Recovery" },
          { _injury: "Addiction", _location: "Recovery" },
          { _injury: "Rehabilitation", _location: "Recovery" },
          { _injury: "Mobility Issues", _location: "Recovery" }
      ];

      // Populate the dropdown
      const injurySelect = document.getElementById("injury");
      data.forEach(item => {
          const option = document.createElement("option");
          option.value = item._injury;
          option.textContent = item._injury;
          injurySelect.appendChild(option);
      });

      // Handle dropdown change event
      injurySelect.addEventListener("change", function () {
          const selectedInjury = this.value;
          const selectedData = data.find(item => item._injury === selectedInjury);

          const locationInput = document.getElementById("location");
          if (selectedData) {
              locationInput.value = selectedData._location;
          } else {
              locationInput.value = ""; // Clear the location if no match
          }
      });
      
  </script>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script type="module">
    var currentUserID = 4;

  import {
    pythonURI,
    fetchOptions,
  } from "{{ site.baseurl }}/assets/js/api/config.js";

  let map;

  document.addEventListener("DOMContentLoaded", () => {
    // Initialize the map
    map = L.map("map").setView([32.7157, -117.1611], 12); // Default to San Diego
    L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
      maxZoom: 18,
      attribution: "© OpenStreetMap contributors",
    }).addTo(map);

    getCareCenterData(currentUserID);

    const goButton = document.getElementById("goButton");
    goButton.addEventListener("click", (event) => {
      event.preventDefault(); // Prevent the form from submitting
      FindLocations();
    });
  });

  async function FindLocations() {
    const location = document
        .getElementById("location")
        .value.trim()
        .replace(/\s+/g, "+"); // Clean up spaces in location input
    const place = document
        .getElementById("place")
        .value.trim()
        .replace(/\s+/g, "+"); // Clean up spaces in place input

    if (!place) {
        alert("Please enter a valid city.");
        return;
    }

    const url = `https://nominatim.openstreetmap.org/search?q=${location}+in+${place}&format=json&addressdetails=1&limit=10`;

    try {
        const response = await fetch(url, {
            headers: {
                "User-Agent": "MyWaypointApp/1.0 (contact@example.com)",
            },
        });

        if (!response.ok) {
            console.error(`HTTP Error: ${response.status}`);
            alert(`Failed to fetch data. Status: ${response.status}`);
            return;
        }

        const data = await response.json();

        // Clear previous results and markers
        const resultsTableBody = document.querySelector("#resultsTable tbody");
        resultsTableBody.innerHTML = ""; // Reset table content
        map.eachLayer((layer) => {
            if (layer instanceof L.Marker) {
                map.removeLayer(layer);
            }
        });

        if (data.length === 0) {
            const noResultsRow = document.createElement("tr");
            noResultsRow.innerHTML = `<td colspan="4">No locations found. Try a different query.</td>`;
            resultsTableBody.appendChild(noResultsRow);
            return;
        }

        data.forEach((place, index) => {
            // Add table row
            const row = document.createElement("tr");
            row.innerHTML = `
                <td>${index + 1}</td>
                <td>${place.display_name.split(",")[0]}</td>
                <td>${place.display_name}</td>
                <td>
                    <button class="like-button" data-title="${place.display_name}">Check In</button>
                </td>
            `;
            resultsTableBody.appendChild(row);

            // Add marker to the map
            const marker = L.marker([place.lat, place.lon]).addTo(map);
            marker.bindPopup(`<b>${place.display_name}</b>`);
        });

        // Add event listeners for like buttons
        document.querySelectorAll(".like-button").forEach((button) => {
            button.addEventListener("click", () => {
                if (button.textContent === "Check In") {
                    const title = button.getAttribute("data-title");
                    checkinCareLocation(title);
                    button.textContent = "Checked In"; // Update button text
                }
            });
        });

        // Adjust map view to fit all markers
        const markers = data.map((place) => [place.lat, place.lon]);
        const bounds = L.latLngBounds(markers);
        map.fitBounds(bounds);
    } catch (error) {
        console.error("Error fetching data:", error);
        alert("An error occurred while fetching data. Please try again later.");
    }
}


  function checkinCareLocation(title) {
    const location = document
      .getElementById("location")
      .value.trim();
    const place = document
      .getElementById("place")
      .value.trim();
    const injury = document
      .getElementById("injury")
      .value.trim();

    postCareCenterData(injury, location, title);

    alert(`Checked in for:${injury} to ${location} \nlocated in city:${place} \nat address:${title}`);   
    console.log(`Checked in for:${injury} to ${location} \nlocated in city:${place} \nat address:${title}`);

    getCareCenterData(currentUserID);

  }

  function checkoutCareLocation(waypointID) {
    deleteCareCenterData(waypointID);
 
    const location = document
      .getElementById("location")
      .value.trim();
    const place = document
      .getElementById("place")
      .value.trim();
    const injury = document
      .getElementById("injury")
      .value.trim();

    console.log(`Discharged for:${injury} from ${location} \nlocated in city:${place}`);
    alert(`Discharged for:${injury} from ${location} \nlocated in city:${place}`);
    
    getCareCenterData(currentUserID);

  }

  function updateRating(waypointId, rating) {

    updateCareCenterData(waypointId, rating);
    alert(`Rating changed to: ${rating}`);
    getCareCenterData(currentUserID);

  }



  async function postCareCenterData(injury, location, address) {

    const postData = {
        injury: injury,
        location: location,
        address: address
    };

  try {
        const response = await fetch(`${pythonURI}/api/waypoints`, {
        ...fetchOptions,
        method: 'POST',
        body: JSON.stringify(postData)
        });

        if (!response.ok) {
        throw new Error(`HTTP error! Status: ${response.status}`);
        }

        const data = await response.json();

        if (typeof data === "object") {
            currentUserID = data.user_id;
        }

      }catch (error) {
        console.error("Error posting data:", error);
      }
  }

async function getCareCenterData(currentUserID) {
    try {
        const response = await fetch(`${pythonURI}/api/waypoints?user_id=${currentUserID}`, {
            ...fetchOptions,
            method: 'GET',
        });

        if (!response.ok) {
            throw new Error(`HTTP error! Status: ${response.status}`);
        }

        const data = await response.json();

        const carecenterTable = document.querySelector("#carecenterTable tbody");
        carecenterTable.innerHTML = ""; // Clear previous entries

        if (data.length === 0) {
            const noResultsRow = document.createElement("tr");
            noResultsRow.innerHTML = `<td colspan="6">No care center check-ins available.</td>`;
            carecenterTable.appendChild(noResultsRow);
            return;
        }

        data.forEach((waypoint, index) => {
            const row = document.createElement("tr");
            row.innerHTML = `
                <td>${index + 1}</td>
                <td>${waypoint.injury}</td>
                <td>${waypoint.location}</td>
                <td>${waypoint.address}</td>
                <td>
                    <div class="ratings">
                        ${[1, 2, 3, 4, 5]
                            .map(
                                (rating) => `
                                    <span 
                                        class="rating-star 
                                            ${rating <= waypoint.rating ? (rating <= 2 ? "red" : rating <= 4 ? "yellow" : "green") : ""}
                                            ${rating <= waypoint.rating ? "active" : ""}" 
                                        data-rating="${rating}" 
                                        data-waypointid="${waypoint.id}">
                                        &#9733;
                                    </span>
                                `
                            )
                            .join("")}
                    </div>
                </td>
                <td>
                    <button class="checkout-button" data-waypointid="${waypoint.id}">Check Out</button>
                </td>
            `;
            carecenterTable.appendChild(row);
        });

        // Add event listeners to "Check Out" buttons
        document.querySelectorAll(".checkout-button").forEach((button) => {
            button.addEventListener("click", () => {
                const waypointId = button.getAttribute("data-waypointid");
                checkoutCareLocation(waypointId);
            });
        });

        // Add event listeners to rating stars
        document.querySelectorAll(".rating-star").forEach((star) => {
            star.addEventListener("click", async (event) => {
                const selectedRating = event.target.getAttribute("data-rating");
                const waypointId = event.target.getAttribute("data-waypointid");
                updateRating(waypointId, selectedRating);
            });
        });
    } catch (error) {
        console.error("Error fetching care center data:", error);
    }
}

async function updateCareCenterData(waypointId, rating) {
    try {
        const response = await fetch(`${pythonURI}/api/waypoints`, {
            ...fetchOptions,
            method: 'PUT',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify({ rating: parseInt(rating), waypoint_id: parseInt(waypointId) })
        });

        if (!response.ok) {
            throw new Error(`HTTP error! Status: ${response.status}`);
        }

        console.log(`Rating updated for waypoint ID ${waypointId} to ${rating} stars.`);
    } catch (error) {
        console.error("Error updating rating:", error.message);
    }
}


async function deleteCareCenterData(waypointId) {
    try {
        const response = await fetch(`${pythonURI}/api/waypoints?waypoint_id=${waypointId}`, {
            ...fetchOptions,
            method: 'DELETE'
        });

        if (!response.ok) {
            throw new Error(`HTTP error! Status: ${response.status}`);
        }

        console.log(`Waypoint ID ${waypointId} deleted successfully.`);
        getCareCenterData(currentUserID);

    } catch (error) {
        console.error("Error deleting care center data:", error.message);
    }
}


</script>
