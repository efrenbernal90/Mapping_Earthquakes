# Mapping_Earthquakes

## Overview:

We are creating a website to display earthquake data across the globe! Using a Mapbox API and GeoJSON data from the USGS website, we can plot earthquake "markers" onto a map using Leaflet. 

### Resources:

#### Data was provided in:

- [USGS Earthquakes GeoJSON](https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_week.geojson)
- [USGS Earthquakes >4.5 Mag GeoJSON](https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/4.5_week.geojson)
- [Techtonic Plate Github Repository JSON](https://raw.githubusercontent.com/fraxen/tectonicplates/master/GeoJSON/PB2002_boundaries.json)


#### Additional Resources: 
- HTML
- JavaScript 
- Leaflet CSS v1.7.1
- Mapbox API
- d3.js v5 (For parsing JSON data)

#### Additional Documentation:

- [JavaScritWebDocs](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
- [Leaflet GeoJSON](https://leafletjs.com/examples/geojson/)
- [Mapbox API/Map Styles](https://docs.mapbox.com/api/maps/styles/?q=Zoom&size=n_10_n)
- [d3Documentation](https://github.com/d3/d3-request/blob/master/README.md)

## Results

Use Leaflet to create map layers, such as a "navigation dark layer": 

```js
let darkNav = L.tileLayer(
  "https://api.mapbox.com/styles/v1/mapbox/navigation-night-v1/tiles/{z}/{x}/{y}?access_token={accessToken}",
  {
    attribution:
      'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, <a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery (c) <a href="https://www.mapbox.com/">Mapbox</a>',
    maxZoom: 18,
    accessToken: API_KEY,
  }
);
```
    
A base layer is needed for page to display default map object layer:

```js
let map = L.map("mapid", {
  center: [40.7, -94.5],
  zoom: 3,
  layers: [streets],
});
```
    
Add base layer to contain all maps:
```js
let baseMaps = {
  Streets: streets,
  Satellite: satelliteStreets,
  Dark: darkNav,
};
```

Add data overlays to map layers, such as techtonic plate data:
```js
let techtonicPlates = new L.LayerGroup();

let overlays = {
  "Techtonic Plates": techtonicPlates,
};
```

Add control to map to allow users to change between layers:

```js
L.control.layers(baseMaps, overlays).addTo(map);
```

Parse the data, create a layer and add it to the map:

```js
d3.json(
  "https://raw.githubusercontent.com/fraxen/tectonicplates/master/GeoJSON/PB2002_boundaries.json"
).then(function (plates) {
  // Creating a GeoJSON layer with the retrieved data, format to visualiz the data.
  L.geoJson(plates, {
    color: "red",
    weight: 2,
  }).addTo(techtonicPlates);

  // Add techtonic plate layer to map
  techtonicPlates.addTo(map);
});
```

### Map Preview

After adding more layers and popup functionality, the map appears like the following:

![Dark_Map](Earthquake_Challenge\static\css\images\darkNav.png)

