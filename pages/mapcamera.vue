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
    zoom: 15.5,
    center: [-87.62712, 41.89033],
    pitch: 45,
    //bearing: 160,
    //interactive: false,
    // Choose from Mapbox's core styles, or make your own style with Mapbox Studio
    style: "mapbox://styles/mapbox/light-v10",
  },
});
function onMapLoaded(map: mapboxgl.Map) {
  function rotateCamera(timestamp) {
    // clamp the rotation between 0 -360 degrees
    // Divide timestamp by 100 to slow rotation to ~10 degrees / sec
    map.rotateTo((timestamp / 100) % 360, { duration: 0 });
    // Request the next frame of the animation.
    requestAnimationFrame(rotateCamera);
  }
  rotateCamera(0);

  // Add 3D buildings and remove label layers to enhance the map
  const layers = map.getStyle().layers;
  for (const layer of layers) {
    if (layer.type === "symbol" && layer.layout["text-field"]) {
      // remove text labels
      map.removeLayer(layer.id);
    }
  }

  map.addLayer({
    id: "3d-buildings",
    source: "composite",
    "source-layer": "building",
    filter: ["==", "extrude", "true"],
    type: "fill-extrusion",
    minzoom: 15,
    paint: {
      "fill-extrusion-color": "#aaa",

      // use an 'interpolate' expression to add a smooth transition effect to the
      // buildings as the user zooms in
      "fill-extrusion-height": [
        "interpolate",
        ["linear"],
        ["zoom"],
        15,
        0,
        15.05,
        ["get", "height"],
      ],
      "fill-extrusion-base": [
        "interpolate",
        ["linear"],
        ["zoom"],
        15,
        0,
        15.05,
        ["get", "min_height"],
      ],
      "fill-extrusion-opacity": 0.6,
    },
  });
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
</style>
