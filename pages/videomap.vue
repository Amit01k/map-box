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
// const state = reactive({
//   map: {
//     container: "map",
//     center: [-122.514426, 37.562984],
//     minZoom: 14,
//     zoom: 17,
//     bearing: -96,
//     style: videoStyle,
//     //bearing: 160,
//     //interactive: false,
//     // Choose from Mapbox's core styles, or make your own style with Mapbox Studio
//     //style: "mapbox://styles/mapbox/light-v10",
//   },
// });
console.log("line num 56");
//onMapLoaded();
function onMapLoaded(map: mapboxgl.Map) {
  console.log("line nu 57");
  const videoStyle = {
    version: 8,
    sources: {
      satellite: {
        type: "raster",
        url: "mapbox://mapbox.satellite",
        tileSize: 256,
      },
      video: {
        type: "video",
        urls: [
          "https://static-assets.mapbox.com/mapbox-gl-js/drone.mp4",
          "https://static-assets.mapbox.com/mapbox-gl-js/drone.webm",
        ],
        coordinates: [
          [-122.51596391201019, 37.56238816766053],
          [-122.51467645168304, 37.56410183312965],
          [-122.51309394836426, 37.563391708549425],
          [-122.51423120498657, 37.56161849366671],
        ],
      },
    },
    layers: [
      {
        id: "background",
        type: "background",
        paint: {
          "background-color": "rgb(4,7,14)",
        },
      },
      {
        id: "satellite",
        type: "raster",
        source: "satellite",
      },
      {
        id: "video",
        type: "raster",
        source: "video",
      },
    ],
  };

  const state = reactive({
    map: {
      container: "map",
      center: [-122.514426, 37.562984],
      minZoom: 14,
      zoom: 17,
      bearing: -96,
      style: videoStyle,
      //bearing: 160,
      //interactive: false,
      // Choose from Mapbox's core styles, or make your own style with Mapbox Studio
      //style: "mapbox://styles/mapbox/light-v10",
    },
  });
  let playingVideo = true;

  console.log("103 ");
  map.on("click", () => {
    playingVideo = !playingVideo;

    if (playingVideo) {
      map.getSource("video").play();
    } else {
      map.getSource("video").pause();
    }
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
