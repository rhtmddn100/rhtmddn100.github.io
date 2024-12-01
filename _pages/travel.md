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
<script src="https://unpkg.com/leaflet.markercluster@1.5.3/dist/leaflet.markercluster.js"></script>
<link rel="stylesheet" href="https://unpkg.com/leaflet.markercluster@1.5.3/dist/MarkerCluster.css" />
<link rel="stylesheet" href="https://unpkg.com/leaflet.markercluster@1.5.3/dist/MarkerCluster.Default.css" />

<script>
  // Initialize the map
  const map = L.map('map').setView([20, 0], 2); // Initial view (latitude, longitude, zoom level)

  // Add a tile layer
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
  }).addTo(map);

  // Define visited countries as polygons (GeoJSON format)
  const countries = [
    {
      "type": "Feature",
      "geometry": {
        "type": "Polygon",
        "coordinates": [[[8.2275, 46.8182], [8.23, 47.818], [9.23, 47.81], [9.2275, 46.8182], [8.2275, 46.8182]]] // Example Switzerland
      },
      "properties": {
        "name": "Switzerland"
      }
    }
  ];

  L.geoJSON(countries, {
    style: { color: 'blue', fillOpacity: 0.4 },
  }).addTo(map);

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