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

  // Add a clean tile layer
  L.tileLayer('https://stamen-tiles.a.ssl.fastly.net/toner-background/{z}/{x}/{y}.png', {
    attribution: '&copy; <a href="https://stamen.com/">Stamen Design</a>',
  }).addTo(map);

  const places = {{ site.data.places.places | jsonify }};

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
      L.marker([city.lat, city.lon])
        .addTo(map)
        .bindPopup(`<b>${city.name}</b>`);
    });
  });
</script>
