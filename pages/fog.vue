<template>
  <div>
    <main class="w-screen h-screen">
      <head>
        <link
          href="https://api.mapbox.com/mapbox-gl-js/v2.10.0/mapbox-gl.css"
          rel="stylesheet"
        />
      </head>
      <v-map class="w-full h-full" :options="state.map" @loaded="onMapLoaded" />
      <div id="menu" class="">
        <input
          id="satellite-v9"
          type="radio"
          name="rtoggle"
          value="satellite"
          checked="checked"
        />
        <label for="satellite-v9">satellite</label>
        <input id="light-v10" type="radio" name="rtoggle" value="light" />
        <label for="light-v10">light</label>
        <input id="dark-v10" type="radio" name="rtoggle" value="dark" />
        <label for="dark-v10">dark</label>
        <input id="streets-v11" type="radio" name="rtoggle" value="streets" />
        <label for="streets-v11">streets</label>
        <input id="outdoors-v11" type="radio" name="rtoggle" value="outdoors" />
        <label for="outdoors-v11">outdoors</label>
      </div>
    </main>
  </div>
</template>
<script setup lang="ts">
import MapboxGeocoder from "@mapbox/mapbox-gl-geocoder";
import vMap from "v-mapbox";
import mapboxgl from "mapbox-gl";
import geojson from "geojson";
import { once } from "events";
//import globe from "globe"
mapboxgl.accessToken =
  "pk.eyJ1Ijoic29jaWFsZXhwbG9yZXIiLCJhIjoiREFQbXBISSJ9.dwFTwfSaWsHvktHrRtpydQ";
//const data: string;
const state = reactive({
  map: {
    container: "map",
    zoom: 13,
    center: [6.6301, 45.35625],
    pitch: 80,
    bearing: 160,
    interactive: false,
    // Choose from Mapbox's core styles, or make your own style with Mapbox Studio
    style: "mapbox://styles/mapbox/satellite-streets-v11",
  },
});
function onMapLoaded(map: mapboxgl.Map) {
  map.setFog({
    range: [-1, 2],
    "horizon-blend": 0.3,
    color: "white",
    "high-color": "#ADD8E6",
    "space-color": "#D8F2FF",
    "star-intensity": 0.0,
  });
  // Add some 3D terrain
  map.addSource("mapbox-dem", {
    type: "raster-dem",
    url: "mapbox://mapbox.terrain-rgb",
    tileSize: 512,
    maxzoom: 14,
  });
  map.setTerrain({
    source: "mapbox-dem",
    exaggeration: 1.5,
  });
  // Run a timing loop to switch between day and night
  map.once("idle");
  let lastTime = 0.0;
  let animationTime = 0.0;
  let cycleTime = 0.0;
  let night = true;
  const initialBearing = map.getBearing();
  function frame(time) {
    const elapsedTime = (time - lastTime) / 1000.0;
    animationTime += elapsedTime;
    cycleTime += elapsedTime;
    if (cycleTime >= 10.0) {
      if (night) {
        // night fog styling
        map.setFog({
          range: [-1, 2],
          "horizon-blend": 0.3,
          color: "#242B4B",
          "high-color": "#161B36",
          "space-color": "#0B1026",
          "star-intensity": 0.8,
        });
      } else {
        // day fog styling
        map.setFog({
          range: [-1, 2],
          "horizon-blend": 0.3,
          color: "white",
          "high-color": "#ADD8E6",
          "space-color": "#D8F2FF",
          "star-intensity": 0.0,
        });
      }
      night = !night;
      cycleTime = 0.0;
    }
    const rotation = initialBearing + animationTime * 2.0;
    map.setBearing(rotation % 360);
    lastTime = time;
    window.requestAnimationFrame(frame);
  }
  window.requestAnimationFrame(frame);
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
/* .marker {
      background-image: url("http://localhost:3000/assets/images/download.jpg");
      background-size: cover;
      width: 50px;
      height: 50px;
      border-radius: 50%;
      cursor: pointer;
    }   */
.mapboxgl-popup {
  max-width: 200px;
}
.mapboxgl-popup-content {
  text-align: center;
  font-family: "Open Sans", sans-serif;
}
#menu {
  position: absolute;
  background: #efefef;
  padding: 10px;
  font-family: "Open Sans", sans-serif;
}
</style>
