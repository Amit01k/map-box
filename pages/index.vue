<template>
  <main class="w-screen h-screen">
    <v-map class="w-full h-full" :options="state.map" @loaded="onMapLoaded" />
    <!-- Amit -->
  </main>
</template>
<script setup lang="ts">
// import "mapbox-gl/dist/mapbox-gl.css";
// import "v-mapbox/dist/v-mapbox.css";
import VMap from "v-mapbox";
import mapboxgl from "mapbox-gl";
const state = reactive({
  map: {
    accessToken:
      "pk.eyJ1Ijoic29jaWFsZXhwbG9yZXIiLCJhIjoiREFQbXBISSJ9.dwFTwfSaWsHvktHrRtpydQ",
    //style: "mapbox://styles/mapbox/streets-v11?optimize=true",
    style: "mapbox://styles/mapbox/satellite-streets-v11",
    // style: "https://basemaps.cartocdn.com/gl/dark-matter-gl-style/style.json",
    center: [444.04931277036667, 26.266912177018096] as number[],
    zoom: 11,
    maxZoom: 22,
  },
});
// mapboxgl.accessToken =
//   "pk.eyJ1Ijoic29jaWFsZXhwbG9yZXIiLCJhIjoiREFQbXBISSJ9.dwFTwfSaWsHvktHrRtpydQ";
// var map = new mapboxgl.Map({
//   container: "map",
//   style: "mapbox://styles/mapbox/streets-v11?optimize=true",
//   center: [444.04931277036667, 26.266912177018096] as number[],
//   zoom: 11,
//   //     maxZoom: 22,
// });

var geojson = {
  type: "FeatureCollection",
  features: [
    {
      type: "Feature",
      properties: { title: "Mapbox", description: "Washington, D.C." },
      geometry: {
        type: "Point",
        coordinates: [444.04931277036667, 26.266912177018096],
      },
    },
    {
      type: "Feature",
      properties: {},
      geometry: {
        type: "Point",
        coordinates: [444.0481862425804, 26.266565820518633],
      },
    },
    {
      type: "Feature",
      properties: {},
      geometry: {
        type: "Point",
        coordinates: [444.0496963262558, 26.266450368122516],
      },
    },
    {
      type: "Feature",
      properties: {},
      geometry: {
        type: "Point",
        coordinates: [444.04630064964294, 26.23214630354235],
      },
    },
  ],
};

function onMapLoaded(map) {
  console.log("amit kumar");
  //new mapboxgl.Marker().setLngLat([444.0463, 26.2321]).addTo(map);
  for (const feature of geojson.features) {
    // create a HTML element for each feature
    const el = document.createElement("div");
    el.className = "marker";

    // make a marker for each feature and add to the map
    new mapboxgl.Marker(el)
      .setLngLat(feature.geometry.coordinates)

      .addTo(map);

    map.on("click", (e) => {
      console.log(`A click event has occurred at ${e.lngLat}`);
    });
  }
}
</script>
<style>
.w-screen {
  width: 100vw;
}

.h-screen {
  height: 100vh;
}

.h-full {
  height: 100%;
}

.w-full {
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
