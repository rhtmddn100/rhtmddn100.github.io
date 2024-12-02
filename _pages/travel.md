---
layout: page
title: Travel
permalink: /travel/
nav: true
nav_order: 7
---

# My Travel Map

<div id="map" style="height: 400px; width: 100%; margin-top: 20px;"></div>

<div id="city-list-container">
  <h3>List of Cities Visited</h3>
  <button id="sort-chronologically">Sort Chronologically</button>
  <button id="sort-by-continent">Sort by Continent</button>
  <ul id="city-list"></ul>
</div>

<style>
  #city-list-container {
    margin-top: 20px;
  }
  #city-list li {
    margin: 5px 0;
  }
</style>

<!-- Include Leaflet.js and its styles -->
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />

<script>
  // Initialize the map
  const map = L.map('map').setView([20, 0], 1); // Initial view (latitude, longitude, zoom level)
  map.setMaxBounds([[85, -180], [-60, 180]]);

  const doubleCircleIcon = L.icon({
    iconUrl: '/assets/img/dc_icon.svg', // Path to your SVG file
    iconSize: [10, 10], // Adjust size as needed
    iconAnchor: [5, 5], // Center the icon
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
            weight: isVisited ? 1 : 0,
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

<script>
// JavaScript code goes here
const renderCityList = (places, sortBy = "chronologically") => {
const cityListElement = document.getElementById("city-list");
cityListElement.innerHTML = ""; // Clear the current list

// Flatten the data into a single array of cities
let cities = places.flatMap((place) =>
    place.cities.map((city) => ({
    ...city,
    country: place.country,
    flag: place.flag,
    continent: place.continent,
    }))
);

if (sortBy === "chronologically") {
    cities.sort((a, b) => new Date(a.start_date) - new Date(b.start_date));
} else if (sortBy === "continent") {
    cities.sort((a, b) => a.continent.localeCompare(b.continent));
}

// Group cities by continent if sorting by continent
if (sortBy === "continent") {
    const continents = [...new Set(cities.map((city) => city.continent))];
    continents.forEach((continent) => {
        const continentHeader = document.createElement("h4");
        continentHeader.innerHTML = `<strong>${continent}</strong>`;
        cityListElement.appendChild(continentHeader);

        cities
            .filter((city) => city.continent === continent)
            .forEach((city) => {
            const listItem = document.createElement("li");
            listItem.innerHTML = `
                <img src="https://flagcdn.com/w40/${city.flag}.png" alt="${city.country}" style="width: 20px; height: 15px; margin-right: 5px;">
                <strong>${city.name}</strong> (${city.country}): ${city.start_date} - ${city.end_date}
            `;
            cityListElement.appendChild(listItem);
            });
    });
} else {
    // Render a simple list for chronological order
    cities.forEach((city) => {
    const listItem = document.createElement("li");
    listItem.innerHTML = `
        <img src="https://flagcdn.com/w40/${city.flag}.png" alt="${city.country}" style="width: 20px; height: 15px; margin-right: 5px;">
        <strong>${city.name}</strong> (${city.country}): ${city.start_date} - ${city.end_date}
    `;
    cityListElement.appendChild(listItem);
    });
}
};

  document.getElementById("sort-chronologically").addEventListener("click", () => {
    renderCityList(places, "chronologically");
  });

  document.getElementById("sort-by-continent").addEventListener("click", () => {
    renderCityList(places, "continent");
  });

  renderCityList(places);
</script>
