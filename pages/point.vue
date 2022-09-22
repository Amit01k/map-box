<template>
  <main class="w-screen h-screen">
    <v-map class="w-full h-full" :options="state.map" @loaded="onMapLoaded" />
    <!-- Amit -->
  </main>
</template>
<script setup lang="ts">
import VMap from "v-mapbox";
import MapboxGeocoder from "@mapbox/mapbox-gl-geocoder";
import "@mapbox/mapbox-gl-geocoder/dist/mapbox-gl-geocoder.css";
//import VMap from "v-mapbox";
import mapboxgl from "mapbox-gl";
const state = reactive({
  map: {
    accessToken:
      "pk.eyJ1Ijoic29jaWFsZXhwbG9yZXIiLCJhIjoiREFQbXBISSJ9.dwFTwfSaWsHvktHrRtpydQ",
    style: "mapbox://styles/mapbox/streets-v11?optimize=true",
    // style: "https://basemaps.cartocdn.com/gl/dark-matter-gl-style/style.json",
    center: [444.04931277036667, 26.266912177018096] as number[],
    zoom: 11,
    maxZoom: 22,
  },
});
var geojson = {
  type: "FeatureCollection",
  features: [
    {
      type: "Feature",
      properties: { title: "amit home ", description: "back to home " },
      geometry: {
        type: "Point",
        coordinates: [444.04931277036667, 26.266912177018096],
      },
    },
    {
      type: "Feature",
      properties: { title: "villege", description: "uttar pradesh" },
      geometry: {
        type: "Point",
        coordinates: [444.0481862425804, 26.266565820518633],
      },
    },
    {
      type: "Feature",
      properties: { title: "market", description: "uttar pradesh." },
      geometry: {
        type: "Polygon",
        //coordinates: [444.0496963262558, 26.266450368122516],
        coordinates: [
          [
            [436.11328125, 24.206889622398023],
            [437.958984375, 24.206889622398023],
            [437.958984375, 25.12539261151203],
            [436.11328125, 25.12539261151203],
            [436.11328125, 24.206889622398023],
          ],
        ],
      },
    },
    {
      type: "Feature",
      properties: { title: "home", description: "asdfghjhjkl." },
      geometry: {
        type: "Point",
        coordinates: [444.04630064964294, 26.23214630354235],
      },
    },
  ],
};

// function onMapLoaded(map: mapboxgl.Map) {
//   console.log("amit kumar");
//   //new mapboxgl.Marker().setLngLat([444.0463, 26.2321]).addTo(map);
//   for (const feature of geojson.features) {
//     // create a HTML element for each feature
//     const el = document.createElement("div");
//     el.className = "marker";

//     // make a marker for each feature and add to the map
//     new mapboxgl.Marker(el)
//       .setLngLat(feature.geometry.coordinates)
//       .setPopup(
//         new mapboxgl.Popup({ offset: 25 }) // add popups
//           .setHTML(
//             `<h3>${feature.properties.title}</h3><p>${feature.properties.description}</p>`
//           )
//       )
//       .addTo(map);
//   }
// }

var Polygon_value = {
  type: "FeatureCollection",
  features: [
    {
      type: "Feature",
      properties: {},
      geometry: {
        type: "Polygon",
        coordinates: [
          [
            [438.3489990234375, 23.74009762440226],
            [438.4039306640625, 23.0999442125314],
            [438.958740234375, 23.266580706342193],
            [439.3487548828125, 23.810475327766568],
            [438.673095703125, 23.865745352647956],
            [438.3489990234375, 23.74009762440226],
          ],
        ],
      },
    },
    {
      type: "Feature",
      properties: {},
      geometry: {
        type: "Polygon",
        coordinates: [
          [
            [431.21337890624994, 23.745125865762923],
            [430.718994140625, 23.200960808078566],
            [431.114501953125, 23.185813175302915],
            [431.751708984375, 23.15551276434146],
            [431.21337890624994, 23.745125865762923],
          ],
        ],
      },
    },
  ],
};

//map.on("load", () => {
  // Add a data source containing GeoJSON data.
  // map.addSource("maine", {
  //   type: "geojson",
  //   data: {
  //     type: "Feature",
  //     geometry: {
  //       type: "Polygon",
  //       // These coordinates outline Maine.
  //       coordinates: [
  //         [
  //           [-67.13734, 45.13745],
  //           [-66.96466, 44.8097],
  //           [-68.03252, 44.3252],
  //           [-69.06, 43.98],
  //           [-70.11617, 43.68405],
  //           [-70.64573, 43.09008],
  //           [-70.75102, 43.08003],
  //           [-70.79761, 43.21973],
  //           [-70.98176, 43.36789],
  //           [-70.94416, 43.46633],
  //           [-71.08482, 45.30524],
  //           [-70.66002, 45.46022],
  //           [-70.30495, 45.91479],
  //           [-70.00014, 46.69317],
  //           [-69.23708, 47.44777],
  //           [-68.90478, 47.18479],
  //           [-68.2343, 47.35462],
  //           [-67.79035, 47.06624],
  //           [-67.79141, 45.70258],
  //           [-67.13734, 45.13745],
  //         ],
  //       ],
  //     },
  //   },
  // });

  // // Add a new layer to visualize the polygon.
  // map.addLayer({
  //   id: "maine",
  //   type: "fill",
  //   source: "maine", // reference the data source
  //   layout: {},
  //   paint: {
  //     "fill-color": "#0080ff", // blue color fill
  //     "fill-opacity": 0.5,
  //   },
  // });
  // // Add a black outline around the polygon.
  // map.addLayer({
  //   id: "outline",
  //   type: "line",
  //   source: "maine",
  //   layout: {},
  //   paint: {
  //     "line-color": "#000",
  //     "line-width": 3,
  //   },
  // });
//});
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
  width: 20px;
  height: 20px;
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
