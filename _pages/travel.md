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
  const map = L.map('map').setView([20, 0], 1); // Initial view (latitude, longitude, zoom level)

  // Add a clean tile layer
  L.tileLayer('https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png', {
    attribution: '&copy; <a href="https://carto.com/">CARTO</a>',
  }).addTo(map);

  // Highlight visited countries using GeoJSON
  fetch('/assets/geojson/countries.geojson') // Adjust the path to your GeoJSON file
    .then(response => response.json())
    .then(data => {
      L.geoJSON(data, {
        style: (feature) => {
          const visitedCountries = ["France", "Switzerland"]; // List of visited countries
          return {
            color: visitedCountries.includes(feature.properties.ADMIN) ? "blue" : "gray",
            weight: 1,
            fillOpacity: visitedCountries.includes(feature.properties.ADMIN) ? 0.6 : 0,
          };
        },
        onEachFeature: (feature, layer) => {
          layer.bindPopup(`<b>${feature.properties.NAME}</b>`);
        },
      }).addTo(map);
    })
    .catch(error => console.error("Error loading GeoJSON:", error));

  // Add markers for visited cities
  const cities = [
    { name: "Zurich", lat: 47.3769, lon: 8.5417 },
    { name: "Paris", lat: 48.8566, lon: 2.3522 },
  ];

  cities.forEach(city => {
    L.marker([city.lat, city.lon])
      .addTo(map)
      .bindPopup(`<b>${city.name}</b>`);
  });
</script>
