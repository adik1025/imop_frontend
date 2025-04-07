---
layout: base
title: SD Districts Viewer 
search_exclude: false
permalink: /districts
---

<link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css"/>
<script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>

<div id="map" style="height: 500px;"></div>

<script type="module">
    import { pythonURI, fetchOptions } from "{{site.baseurl}}/assets/js/api/config.js";

    var map = L.map('map').setView([33.014529067894905, -117.12148599570064], 15);

    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      maxZoom: 19,
      attribution: 'OpenStreetMap'
    }).addTo(map);

    fetch(`${pythonURI}/api/districts/get`, fetchOptions)
      .then(response => response.json())
      .then(geojsonData => {
        L.geoJSON(geojsonData, {
          pointToLayer: (feature, latlng) => {
            return L.marker(latlng);
          },
          onEachFeature: (feature, layer) => {
            const name = feature.properties?.name || 'Unnamed District';
            const website = feature.properties?.website || '#';
            const popupContent = `
            <p>${name}</p>
            <a href="${website}" target="_blank" style="text-decoration: none; color: inherit;" onmouseover="this.style.textDecoration='underline'" onmouseout="this.style.textDecoration='none'">Visit Website</a>`;
            layer.bindPopup(popupContent);
          }
        }).addTo(map);
      })
      .catch(error => console.error('Error loading GeoJSON:', error));
</script>
