<template>
  <div>
    <main class="w-screen h-screen">
      <v-map class="w-full h-full" :options="map" @loaded="onMapLoaded" />
    </main>
    <div id="map"></div>
  </div>
</template>
<script>
import VMap from "v-mapbox";
import mapboxgl from "mapbox-gl";
// mapboxgl.accessToken =
//   '"pk.eyJ1Ijoic29jaWFsZXhwbG9yZXIiLCJhIjoiREFQbXBISSJ9.dwFTwfSaWsHvktHrRtpydQ"';

const geojson = {
  type: "FeatureCollection",
  features: [
    {
      type: "Feature",
      geometry: {
        type: "Point",
        coordinates: [-77.032, 38.913],
      },
      properties: {
        title: "Mapbox",
        description: "Washington, D.C.",
      },
    },
    {
      type: "Feature",
      geometry: {
        type: "Point",
        coordinates: [-122.414, 37.776],
      },
      properties: {
        title: "Mapbox",
        description: "San Francisco, California",
      },
    },
  ],
};
// const state = reactive({
//   map: {
//     accessToken:
//       "pk.eyJ1Ijoic29jaWFsZXhwbG9yZXIiLCJhIjoiREFQbXBISSJ9.dwFTwfSaWsHvktHrRtpydQ",
//     style: "mapbox://styles/mapbox/streets-v11?optimize=true",
//     // style: "https://basemaps.cartocdn.com/gl/dark-matter-gl-style/style.json",
//     center: [444.04931277036667, 26.266912177018096],
//     zoom: 11,
//     maxZoom: 22,
//   },
// });

// const map = new mapboxgl.Map({
//   container: "map",
//   style: "mapbox://styles/mapbox/light-v10",
//   center: [-96, 37.8],
//   zoom: 3,
// });
function onMapLoaded(map) {
  console.log("amitdhfuhgvudfnbgfdnb");
  // add markers to map
  for (const feature of geojson.features) {
    // create a HTML element for each feature
    const el = document.createElement("div");
    el.className = "marker";

    // make a marker for each feature and add it to the map
    new mapboxgl.Marker(el)
      .setLngLat(feature.geometry.coordinates)
      .setPopup(
        new mapboxgl.Popup({ offset: 25 }) // add popups
          .setHTML(
            `<h3>${feature.properties.title}</h3><p>${feature.properties.description}</p>`
          )
      )
      .addTo(map);
  }
}
</script>

<style scoped>
body {
  margin: 0;
  padding: 0;
}

#map {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 100%;
}

.marker {
  background-image: url("http://localhost:3000/assets/images/download.png");
  background-size: cover;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  cursor: pointer;
}

.mapboxgl-popup {
  max-width: 200px;
}

.mapboxgl-popup-content {
  text-align: center;
  font-family: "Open Sans", sans-serif;
}
</style>
