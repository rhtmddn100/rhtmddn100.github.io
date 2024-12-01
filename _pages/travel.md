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
  map.setMaxBounds([[90, -180], [-90, 180]]);

  const doubleCircleIcon = L.icon({
    iconUrl: '/assets/img/dc_icon.svg', // Path to your SVG file
    iconSize: [20, 20], // Adjust size as needed
    iconAnchor: [10, 10], // Center the icon
    popupAnchor: [0, -10], // Position the popup
  });

  // Add a clean tile layer
  L.tileLayer('https://{s}.basemaps.cartocdn.com/light_nolabels/{z}/{x}/{y}{r}.png', {
    attribution: '&copy; <a href="https://carto.com/">CARTO</a>',
    subdomains: 'abcd',
    maxZoom: 19,
  }).addTo(map);

  const places = {{ site.data.travel.places | jsonify }};

  // Highlight visited countries using GeoJSON
  fetch('/assets/geojson/countries.geojson') // Adjust the path to your GeoJSON file
    .then(response => response.json())
    .then(data => {
      L.geoJSON(data, {
        style: (feature) => {
          const visitedCountries = places.map(place => place.country);
          const countryName = feature.properties.ADMIN;
          const isVisited = visitedCountries.includes(countryName);
          return {
            color: isVisited ? "blue" : "gray",
            weight: 1,
            fillOpacity: isVisited ? 0.6 : 0,
          };
        },
        onEachFeature: (feature, layer) => {
          layer.bindPopup(`<b>${feature.properties.ADMIN}</b>`);
        },
      }).addTo(map);
    })
    .catch(error => console.error("Error loading GeoJSON:", error));

  // Add markers for visited cities
  places.forEach(place => {
    place.cities.forEach(city => {
      L.marker([city.lat, city.lon], { icon: doubleCircleIcon })
        .addTo(map)
        .bindPopup(`<b>${city.name}</b>`);
    });
  });
</script>
