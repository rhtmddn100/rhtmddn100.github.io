---
layout: page
title: Travel
permalink: /travel/
---

# My Travel Map

<div id="map" style="height: 600px; width: 100%; margin-top: 20px;"></div>

<!-- Include Leaflet.js and its styles -->
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />

<script>
  // Initialize the map
  const map = L.map('map').setView([20, 0], 2); // Initial view (latitude, longitude, zoom level)

  // Add a tile layer for clean borders
  L.tileLayer('https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png', {
    attribution: '&copy; <a href="https://carto.com/">CARTO</a>',
  }).addTo(map);

  // Load GeoJSON country borders
  fetch('/assets/geojson/ne_110m_admin_0_countries.geojson')
    .then(response => response.json())
    .then(data => {
      L.geoJSON(data, {
        style: (feature) => {
          // Highlight visited countries in a different color
          const visitedCountries = ["France", "Switzerland"]; // Update this list with your visited countries
          return {
            color: visitedCountries.includes(feature.properties.NAME) ? "blue" : "gray",
            weight: 1,
            fillOpacity: visitedCountries.includes(feature.properties.NAME) ? 0.6 : 0,
          };
        },
        onEachFeature: (feature, layer) => {
          // Add a popup with the country name
          layer.bindPopup(`<b>${feature.properties.NAME}</b>`);
        },
      }).addTo(map);
    })
    .catch(error => console.error("Error loading GeoJSON:", error));
</script>
