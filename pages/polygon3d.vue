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
    zoom: 14,
    center: [-87.61694, 41.86625],
    //pitch: 45,
    // pitch: 85,
    // bearing: 80,
    // Choose from Mapbox's core styles, or make your own style with Mapbox Studio
    style: "mapbox://styles/mapbox/cjaudgl840gn32rnrepcb9b9g",
    zoom: 15.99,
    pitch: 40,
    bearing: 20,
    antialias: true,
  },
});
function onMapLoaded(map: mapboxgl.Map) {
  map.addSource("floorplan", {
    type: "geojson",
    /*
     * Each feature in this GeoJSON file contains values for
     * `properties.height`, `properties.base_height`,
     * and `properties.color`.
     * In `addLayer` you will use expressions to set the new
     * layer's paint properties based on these values.
     */
    data: "https://docs.mapbox.com/mapbox-gl-js/assets/indoor-3d-map.geojson",
  });
  map.addLayer({
    id: "room-extrusion",
    type: "fill-extrusion",
    source: "floorplan",
    paint: {
      // Get the `fill-extrusion-color` from the source `color` property.
      "fill-extrusion-color": ["get", "color"],

      // Get `fill-extrusion-height` from the source `height` property.
      "fill-extrusion-height": ["get", "height"],

      // Get `fill-extrusion-base` from the source `base_height` property.
      "fill-extrusion-base": ["get", "base_height"],

      // Make extrusions slightly opaque to see through indoor walls.
      "fill-extrusion-opacity": 0.5,
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
#fly {
  display: block;
  position: relative;
  margin: 0px auto;
  width: 50%;
  height: 40px;
  padding: 10px;
  border: none;
  border-radius: 3px;
  font-size: 12px;
  text-align: center;
  color: #fff;
  background: #ee8a65;
}
</style>
