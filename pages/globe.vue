<template>
  <head>
    <link
      rel="stylesheet"
      href="https://api.mapbox.com/mapbox-gl-js/plugins/mapbox-gl-directions/v4.1.0/mapbox-gl-directions.css"
      type="text/css"
    />
    <!-- <link
      href="https://api.mapbox.com/mapbox-gl-js/v2.10.0/mapbox-gl.css"
      rel="stylesheet"
    /> -->
  </head>
  <div>
    <main class="w-screen h-screen">
      <h3>Click here to change language</h3>
      <div class="overlay">
        <button id="replay">Replay</button>
      </div>

      <div>
        <ul id="buttons">
          <li id="button-fr" class="button">French</li>
          <li id="button-ru" class="button">Russian</li>
          <li id="button-de" class="button">German</li>
          <li id="button-es" class="button">Spanish</li>
        </ul>
        <!-- <button id="btn-spin">Pause rotation</button> -->
      </div>
      <v-map class="w-full h-full" :options="state.map" @loaded="onMapLoaded" />
      <button id="btn-spin">Pause rotation</button>
      <div id="menu" class="">
        <input
          id="satellite-v9"
          type="radio"
          name="rtoggle"
          value="satellite"
          checked="checked"
        />
        <!-- See a list of Mapbox-hosted public styles at -->
        <!-- https://docs.mapbox.com/api/maps/styles/#mapbox-styles -->
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
      <!-- <div>
        <ul id="buttons">
          <li id="button-fr" class="button">French</li>
          <li id="button-ru" class="button">Russian</li>
          <li id="button-de" class="button">German</li>
          <li id="button-es" class="button">Spanish</li>
        </ul>
      </div> -->
      <!-- Amit -->
    </main>
    <!-- <p>{{ state.data }}</p>

    <p>below data come from value variable</p>
    <p>{{ value }}</p> -->
  </div>
</template>
<script setup lang="ts">
// import "mapbox-gl/dist/mapbox-gl.css";
// import "v-mapbox/dist/v-mapbox.css";
import MapboxDraw from "@mapbox/mapbox-gl-draw";
import "@mapbox/mapbox-gl-draw/dist/mapbox-gl-draw.css";
import MapboxDirection from "@mapbox/mapbox-gl-directions/dist/mapbox-gl-directions";
//import turf from "@turf/turf";
import MapboxGeocoder from "@mapbox/mapbox-gl-geocoder";
import "@mapbox/mapbox-gl-geocoder/dist/mapbox-gl-geocoder.css";
import VMap from "v-mapbox";
import mapboxgl from "mapbox-gl";

mapboxgl.accessToken =
  "pk.eyJ1Ijoic29jaWFsZXhwbG9yZXIiLCJhIjoiREFQbXBISSJ9.dwFTwfSaWsHvktHrRtpydQ";

//const data: string;
const state = reactive({
  map: {
    container: "map",
    // accessToken:
    //   "pk.eyJ1Ijoic29jaWFsZXhwbG9yZXIiLCJhIjoiREFQbXBISSJ9.dwFTwfSaWsHvktHrRtpydQ",
    //style: "mapbox://styles/mapbox/streets-v11?optimize=true",
    style: "mapbox://styles/mapbox/satellite-streets-v11",
    // style: "https://basemaps.cartocdn.com/gl/dark-matter-gl-style/style.json",
    center: [441.5625, 25.799891182088334] as number[],
    zoom: 2.5,
    maxZoom: 22,
    cooperativeGestures: true,
    projection: "globe",
  },
  data: [],
  polygon: [],
  lineStringData: [],
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
        type: "Point",
        coordinates: [444.0496963262558, 26.266450368122516],
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

function onMapLoaded(map: mapboxgl.Map) {
  getGisData();
  async function getGisData() {
    state.data = await $fetch("http://localhost:8080/gisdata/data");
    console.log("data: ", state.data);
    for (const feature of state.data) {
      // create a HTML element for each feature
      const el = document.createElement("div");
      el.className = "marker";

      console.log("data from databse" + feature.location);

      // make a marker for each feature and add to the map
      new mapboxgl.Marker(el)
        .setLngLat(feature.location.coordinates)
        .setPopup(
          new mapboxgl.Popup({ offset: 20 }) // add popups
            .setHTML(`<h3>${feature.City_Name}</h3><p>${feature.City_Name}</p>`)
        )
        .addTo(map);
      console.log("line 138");
    }
  }

  map.addControl(
    new MapboxDirection({
      accessToken: mapboxgl.accessToken,
    }),
    "top-left"
  );
  //show markers according coordinates

  //draw tool
  var Draw = new MapboxDraw();
  map.addControl(Draw, "top-left");

  //add markers

  map.on("dblclick", (e) => {
    new mapboxgl.Marker({
      color: "#" + (Math.random().toString(16) + "87CEEB").substring(2, 8),
      draggable: true,
    })
      .setLngLat([e.lngLat.lng, e.lngLat.lat])
      .addTo(map);
    console.log(`A click event has occurred at ${e.lngLat}`);
  });

  const layerList = document.getElementById("menu");
  const inputs = layerList.getElementsByTagName("input");

  //dropdown function here

  for (const input of inputs) {
    input.onclick = (layer) => {
      const layerId = layer.target.id;
      map.setStyle("mapbox://styles/mapbox/" + layerId);
    };
  }

  const marker = new mapboxgl.Marker({
    color: "#FFFFFF",
    //draggable: true,
  })
    .setLngLat([30.5, 50.5])
    .addTo(map);

  map.addControl(
    new MapboxGeocoder({
      accessToken: mapboxgl.accessToken,
      mapboxgl: mapboxgl,
    })
  );
  // }
  map.addControl(new mapboxgl.NavigationControl(), "top-left");

  //Add a fullscreen control to a map
  map.addControl(new mapboxgl.FullscreenControl(), "bottom-left");

  // map.addControl(
  //   new MapboxDirection({
  //     accessToken: mapboxgl.accessToken,
  //   }),
  //   "top-left"
  // );

  //map.addControl(new mapboxgl.NavigationControl());

  console.log("line no-190");
  ////////////////////////////////try to add layers on map box///////////////////
  map.addSource("Nagpur", {
    type: "geojson",
    data: {
      type: "Feature",
      geometry: {
        type: "Polygon",
        //<----boundary co-ordinate----->
        coordinates: [
          [
            [431.2188720703125, 23.75518176611264],
            [430.762939453125, 23.17066382710224],
            [431.729736328125, 23.160563309048314],
            [431.2188720703125, 23.75518176611264],
          ],
        ],
      },
    },
  });

  map.addLayer({
    id: "Nagpur",
    type: "fill",
    source: "Nagpur",
    layout: {},
    paint: {
      "fill-color": "#f02",
      //rediish
      "fill-opacity": 0.5,
    },
  });

  map.addLayer({
    id: "outline",
    type: "line",
    source: "Nagpur",
    layout: {},
    paint: {
      "line-color": "#000",
      //dark blackish
      "line-width": 3,
    },
  });
  polygon();
  async function polygon() {
    state.polygon = await $fetch("http://localhost:8080/gisdata/polygon");
    console.log(state.polygon);
    console.log("data: ", state.polygon);

    const featureCollection = {
      type: "geojson",
      data: {
        type: "FeatureCollection",
        features: [],
      },
    };

    featureCollection.data.features = state.polygon.map((element) => {
      return {
        type: "Feature",
        geometry: {
          type: "Polygon",
          coordinates: element.polygon.coordinates,
        },
      };
    });

    map.addSource("polygon-data", featureCollection);

    map.addLayer({
      id: "park-boundary",
      type: "fill",
      source: "polygon-data",
      paint: {
        "fill-color": "#A020F0",
        "fill-opacity": 0.4,
      },
    });

    map.addLayer({
      id: "outline",
      type: "line",
      source: "park-boundary",
      layout: {},
      paint: {
        "line-color": "#000",
        //dark blackish
        "line-width": 3,
      },
    });

    //make a marker for each feature and add to the map
    // new mapboxgl.Marker(el)
    //   .setLngLat(poly.polygon.coordinates)
    //   .setPopup(
    //     new mapboxgl.Popup({ offset: 20 }) // add popups
    //       .setHTML(`<h3>${poly.City_Name}</h3><p>${poly.City_Name}</p>`)
    //   )
    //   .addTo(map);
    console.log("line 138");
    // }
  }

  line();
  async function line() {
    state.lineStringData = await $fetch(
      "http://localhost:8080/gisdata/linestring"
    );

    console.table("line stating data", state.lineStringData);
    const featureCollection = {
      type: "geojson",
      data: {
        type: "FeatureCollection",
        features: [],
      },
    };

    featureCollection.data.features = state.polygon.map((element) => {
      return {
        type: "Feature",
        geometry: {
          type: "MultiLineString",
          coordinates: element.lineStringData.coordinates,
        },
      };
    });
  }

  //change map language
  document.getElementById("buttons").addEventListener("click", (event) => {
    const language = event.target.id.substr("button-".length);
    // Use setLayoutProperty to set the value of a layout property in a style layer.
    // The three arguments are the id of the layer, the name of the layout property,
    // and the new property value.
    map.setLayoutProperty("country-label", "text-field", [
      "get",
      `name_${language}`,
    ]);
  });

  //Create a rotating globe

  map.on("style.load", () => {
    map.setFog({}); // Set the default atmosphere style
  });

  //map.setFog({}); // Set the default atmosphere style

  // The following values can be changed to control rotation speed:

  // At low zooms, complete a revolution every two minutes.
  const secondsPerRevolution = 120;
  // Above zoom level 5, do not rotate.
  const maxSpinZoom = 5;
  // Rotate at intermediate speeds between zoom levels 3 and 5.
  const slowSpinZoom = 3;

  let userInteracting = false;
  let spinEnabled = true;

  function spinGlobe() {
    const zoom = map.getZoom();
    if (spinEnabled && !userInteracting && zoom < maxSpinZoom) {
      let distancePerSecond = 360 / secondsPerRevolution;
      if (zoom > slowSpinZoom) {
        // Slow spinning at higher zooms
        const zoomDif = (maxSpinZoom - zoom) / (maxSpinZoom - slowSpinZoom);
        distancePerSecond *= zoomDif;
      }
      const center = map.getCenter();
      center.lng -= distancePerSecond;
      // Smoothly animate the map over one second.
      // When this animation is complete, it calls a 'moveend' event.
      map.easeTo({ center, duration: 1000, easing: (n) => n });
    }
  }

  // Pause spinning on interaction
  map.on("mousedown", () => {
    userInteracting = true;
  });

  // Restart spinning the globe when interaction is complete
  map.on("mouseup", () => {
    userInteracting = false;
    spinGlobe();
  });

  // These events account for cases where the mouse has moved
  // off the map, so 'mouseup' will not be fired.
  map.on("dragend", () => {
    userInteracting = false;
    spinGlobe();
  });
  map.on("pitchend", () => {
    userInteracting = false;
    spinGlobe();
  });
  map.on("rotateend", () => {
    userInteracting = false;
    spinGlobe();
  });

  // When animation is complete, start spinning if there is no ongoing interaction
  map.on("moveend", () => {
    spinGlobe();
  });

  document.getElementById("btn-spin").addEventListener("click", (e) => {
    spinEnabled = !spinEnabled;
    if (spinEnabled) {
      spinGlobe();
      e.target.innerHTML = "Pause rotation";
    } else {
      map.stop(); // Immediately end ongoing animation
      e.target.innerHTML = "Start rotation";
    }
  });

  spinGlobe();

  // Add an animated point on the map
  map.on("style.load", () => {
    map.setFog({ "horizon-blend": 0.05 }); // Enable stars with reduced atmosphere
  });

  // San Francisco
  const origin = [-122.414, 37.776];

  // Washington DC
  const destination = [-77.032, 38.913];

  // A simple line from origin to destination.
  const route = {
    type: "FeatureCollection",
    features: [
      {
        type: "Feature",
        geometry: {
          type: "LineString",
          coordinates: [origin, destination],
        },
      },
    ],
  };

  // A single point that animates along the route.
  // Coordinates are initially set to origin.
  const point = {
    type: "FeatureCollection",
    features: [
      {
        type: "Feature",
        properties: {},
        geometry: {
          type: "Point",
          coordinates: origin,
        },
      },
    ],
  };

  // Calculate the distance in kilometers between route start/end point.
  const lineDistance = turf.length(route.features[0]);

  const arc = [];

  // Number of steps to use in the arc and animation, more steps means
  // a smoother arc and animation, but too many steps will result in a
  // low frame rate
  const steps = 500;

  // Draw an arc between the `origin` & `destination` of the two points
  for (let i = 0; i < lineDistance; i += lineDistance / steps) {
    const segment = turf.along(route.features[0], i);
    arc.push(segment.geometry.coordinates);
  }

  // Update the route with calculated arc coordinates
  route.features[0].geometry.coordinates = arc;

  // Used to increment the value of the point measurement against the route.
  let counter = 0;

  map.on("load", () => {
    // Add a source and layer displaying a point which will be animated in a circle.
    map.addSource("route", {
      type: "geojson",
      data: route,
    });

    map.addSource("point", {
      type: "geojson",
      data: point,
    });

    map.addLayer({
      id: "route",
      source: "route",
      type: "line",
      paint: {
        "line-width": 2,
        "line-color": "#007cbf",
      },
    });

    map.addLayer({
      id: "point",
      source: "point",
      type: "symbol",
      layout: {
        // This icon is a part of the Mapbox Streets style.
        // To view all images available in a Mapbox style, open
        // the style in Mapbox Studio and click the "Images" tab.
        // To add a new image to the style at runtime see
        // https://docs.mapbox.com/mapbox-gl-js/example/add-image/
        "icon-image": "airport-15",
        "icon-size": 2,
        "icon-rotate": ["get", "bearing"],
        "icon-rotation-alignment": "map",
        "icon-allow-overlap": true,
        "icon-ignore-placement": true,
      },
    });

    function animate() {
      const start =
        route.features[0].geometry.coordinates[
          counter >= steps ? counter - 1 : counter
        ];
      const end =
        route.features[0].geometry.coordinates[
          counter >= steps ? counter : counter + 1
        ];
      if (!start || !end) return;

      // Update point geometry to a new position based on counter denoting
      // the index to access the arc
      point.features[0].geometry.coordinates =
        route.features[0].geometry.coordinates[counter];

      // Calculate the bearing to ensure the icon is rotated to match the route arc
      // The bearing is calculated between the current point and the next point, except
      // at the end of the arc, which uses the previous point and the current point
      point.features[0].properties.bearing = turf.bearing(
        turf.point(start),
        turf.point(end)
      );

      // Update the source with this new data
      map.getSource("point").setData(point);

      // Request the next frame of animation as long as the end has not been reached
      if (counter < steps) {
        requestAnimationFrame(animate);
      }

      counter = counter + 1;
    }

    document.getElementById("replay").addEventListener("click", () => {
      // Set the coordinates of the original point back to origin
      point.features[0].geometry.coordinates = origin;

      // Update the source layer
      map.getSource("point").setData(point);

      // Reset the counter
      counter = 0;

      // Restart the animation
      animate(counter);
    });

    // Start the animation
    animate(counter);
  });
  /////////////////////////////////////////////////////
}
</script>
<style>
.map {
  margin: 0px;
  padding: 0px;
}
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
#menu {
  position: absolute;
  background: #efefef;
  padding: 10px;
  font-family: "Open Sans", sans-serif;
}
#buttons {
  width: 90%;
  margin: 0 auto;
}
.button {
  display: inline-block;
  position: relative;
  cursor: pointer;
  width: 20%;
  padding: 8px;
  border-radius: 3px;
  /* margin-top: 10px; */
  margin: 10px;
  font-size: 12px;
  text-align: center;
  color: #fff;
  background: #ee8a65;
  font-family: sans-serif;
  font-weight: bold;
}
h3 {
  color: rebeccapurple;
  text-align: center;
  margin: 0px;
  padding: 0px;
}
#btn-spin {
  font: bold 12px/20px "Helvetica Neue", Arial, Helvetica, sans-serif;
  background-color: #3386c0;
  color: #fff;
  position: absolute;
  top: 20px;
  left: 50%;
  z-index: 1;
  border: none;
  width: 200px;
  margin-left: -100px;
  display: block;
  cursor: pointer;
  padding: 10px 20px;
  border-radius: 3px;
  margin-top: 65px;
}
#btn-spin:hover {
  background-color: #4ea0da;
}

.overlay {
  position: absolute;
  top: 10px;
  left: 10px;
}

.overlay button {
  font: 600 12px/20px "Helvetica Neue", Arial, Helvetica, sans-serif;
  background-color: #3386c0;
  color: #fff;
  display: inline-block;
  margin: 0;
  padding: 10px 20px;
  border: none;
  cursor: pointer;
  border-radius: 3px;
}

.overlay button:hover {
  background-color: #4ea0da;
}
</style>
