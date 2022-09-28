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
    style: "mapbox://styles/examples/cj68bstx01a3r2rndlud0pwpv",
    zoom: 15,
    pitch: 55,
    bearing: 20,
    antialias: true,
  },
});
function onMapLoaded(map: mapboxgl.Map) {
  const bins = 16;
  const maxHeight = 200;
  const binWidth = maxHeight / bins;

  // Divide the buildings into 16 bins based on their true height, using a layer filter.
  for (let i = 0; i < bins; i++) {
    map.addLayer({
      id: `3d-buildings-${i}`,
      source: "composite",
      "source-layer": "building",
      filter: [
        "all",
        ["==", "extrude", "true"],
        [">", "height", i * binWidth],
        ["<=", "height", (i + 1) * binWidth],
      ],
      type: "fill-extrusion",
      minzoom: 15,
      paint: {
        "fill-extrusion-color": "#aaa",
        "fill-extrusion-height-transition": {
          duration: 0,
          delay: 0,
        },
        "fill-extrusion-opacity": 0.6,
      },
    });
  }

  // Older browsers might not implement mediaDevices at all, so we set an empty object first
  if (navigator.mediaDevices === undefined) {
    navigator.mediaDevices = {};
  }

  // Some browsers partially implement mediaDevices. We can't just assign an object
  // with getUserMedia as it would overwrite existing properties.
  // Here, we will just add the getUserMedia property if it's missing.
  if (navigator.mediaDevices.getUserMedia === undefined) {
    navigator.mediaDevices.getUserMedia = (constraints) => {
      // First get ahold of the legacy getUserMedia, if present
      const getUserMedia =
        navigator.webkitGetUserMedia || navigator.mozGetUserMedia;

      // Some browsers just don't implement it - return a rejected promise with an error
      // to keep a consistent interface
      if (!getUserMedia) {
        return Promise.reject(
          new Error("getUserMedia is not implemented in this browser")
        );
      }

      // Otherwise, wrap the call to the old navigator.getUserMedia with a Promise
      return new Promise((resolve, reject) => {
        getUserMedia.call(navigator, constraints, resolve, reject);
      });
    };
  }

  navigator.mediaDevices
    .getUserMedia({ audio: true })
    .then((stream) => {
      // Set up a Web Audio AudioContext and AnalyzerNode, configured to return the
      // same number of bins of audio frequency data.
      const audioCtx = new (window.AudioContext || window.webkitAudioContext)();

      const analyser = audioCtx.createAnalyser();
      analyser.minDecibels = -90;
      analyser.maxDecibels = -10;
      analyser.smoothingTimeConstant = 0.85;

      const source = audioCtx.createMediaStreamSource(stream);
      source.connect(analyser);

      analyser.fftSize = bins * 2;

      const dataArray = new Uint8Array(bins);

      function draw(now) {
        analyser.getByteFrequencyData(dataArray);

        // Use that data to drive updates to the fill-extrusion-height property.
        let avg = 0;
        for (let i = 0; i < bins; i++) {
          avg += dataArray[i];
          map.setPaintProperty(
            `3d-buildings-${i}`,
            "fill-extrusion-height",
            10 + 4 * i + dataArray[i]
          );
        }
        avg /= bins;

        // Animate the map bearing and light color over time, and make the light more
        // intense when the audio is louder.
        map.setBearing(now / 500);
        const hue = (now / 100) % 360;
        const saturation = Math.min(50 + avg / 4, 100);
        map.setLight({
          color: `hsl(${hue},${saturation}%,50%)`,
          intensity: Math.min(1, (avg / 256) * 10),
        });

        requestAnimationFrame(draw);
      }

      requestAnimationFrame(draw);
    })
    .catch((err) => {
      console.log("The following gUM error occurred:", err);
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
