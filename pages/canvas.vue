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
      <canvas id="canvasID" width="400" height="400"
        >Canvas not supported</canvas
      >
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
//import globe from "globe"
mapboxgl.accessToken =
  "pk.eyJ1Ijoic29jaWFsZXhwbG9yZXIiLCJhIjoiREFQbXBISSJ9.dwFTwfSaWsHvktHrRtpydQ";
//const data: string;
const src = "javascript/game.js";
const state = reactive({
  map: {
    container: "map",
    zoom: 5,
    minZoom: 4,
    center: [95.8991, 18.0887],
    // Choose from Mapbox's core styles, or make your own style with Mapbox Studio
    style: "mapbox://styles/mapbox/streets-v11",
  },
});

function onMapLoaded(map: mapboxgl.Map) {
  const canvas = document.getElementById("canvasID");
  const ctx = canvas.getContext("2d");
  const circles = [];
  const radius = 20;
  if (ctx) {
    canvas.style.display = "none";
  }
  function Circle(x, y, dx, dy, radius, color) {
    this.x = x;
    this.y = y;
    this.dx = dx;
    this.dy = dy;
    this.radius = radius;
    this.draw = function () {
      ctx.beginPath();
      ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2, false);
      ctx.strokeStyle = color;
      ctx.stroke();
    };
    this.update = function () {
      if (this.x + this.radius > 400 || this.x - this.radius < 0) {
        this.dx = -this.dx;
      }
      if (this.y + this.radius > 400 || this.y - this.radius < 0) {
        this.dy = -this.dy;
      }
      this.x += this.dx;
      this.y += this.dy;
      this.draw();
    };
  }
  for (let i = 0; i < 5; i++) {
    const color =
      "#" + (0x1000000 + Math.random() * 0xffffff).toString(16).substr(1, 6);
    const x = Math.random() * (400 - radius * 2) + radius;
    const y = Math.random() * (400 - radius * 2) + radius;
    const dx = (Math.random() - 0.5) * 2;
    const dy = (Math.random() - 0.5) * 2;
    circles.push(new Circle(x, y, dx, dy, radius, color));
  }
  function animate() {
    requestAnimationFrame(animate);
    ctx.clearRect(0, 0, 400, 400);
    for (let r = 0; r < 5; r++) {
      circles[r].update();
    }
  }
  animate();
  map.addSource("canvas-source", {
    type: "canvas",
    canvas: "canvasID",
    coordinates: [
      [91.4461, 21.5006],
      [100.3541, 21.5006],
      [100.3541, 13.9706],
      [91.4461, 13.9706],
    ],
    // Set to true if the canvas source is animated. If the canvas is static, animate should be set to false to improve performance.
    animate: true,
  });
  map.addLayer({
    id: "canvas-layer",
    type: "raster",
    source: "canvas-source",
  });

  console.log("line nu 128");
}
</script>
<style > 
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
