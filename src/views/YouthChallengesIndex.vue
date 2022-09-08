<script>
/* global mapboxgl */
import Darkmode from "darkmode-js";
import axios from "axios";
export default {
  data: function () {
    return {
      message: "",
      youthChallenges: [],
      currentYouthChallenge: {},
      search: "",
    };
  },
  mounted: function () {
    this.mapFun();
    this.darkmode;
  },
  created: function () {
    this.indexYouthChallenges();
  },
  methods: {
    indexYouthChallenges: function () {
      axios.get("/youth_challenges.json").then((response) => {
        this.youthChallenges = response.data;
        console.log(this.youthChallenges);
      });
    },
    filterSearch: function () {
      return this.youthChallenges.filter((youthChallenge) => {
        var lowerSearch = youthChallenge.state.toLowerCase();
        var lowerSearchFilter = this.search.toLowerCase();
        return lowerSearch.includes(lowerSearchFilter);
      });
    },
    mapFun: function () {
      mapboxgl.accessToken = process.env.VUE_APP_MY_API_KEY;
      const map = new mapboxgl.Map({
        container: "map", // container ID
        style: "mapbox://styles/mapbox/streets-v11", // style URL
        center: [-84.388, 33.749], // starting position [lng, lat]
        zoom: 9, // starting zoom
        projection: "globe", // display the map as a 3D globe
      });

      map.on("style.load", () => {
        map.setFog({}); // Set the default atmosphere style
      });

      // create the popup
      const popup = new mapboxgl.Popup({ offset: 25 }).setText("Come enjoy the World of Coca-Cola.");
      // Create a default Marker and add it to the map.
      const marker1 = new mapboxgl.Marker().setLngLat([-84.3924, 33.7626]).setPopup(popup).addTo(map);
      console.log(marker1);
    },
    darkmode: function () {
      new Darkmode().showWidget();
    },
  },
};
</script>

<template>
  <h1>{{ message }}</h1>
  <div class="container">
    <div class="SearchBar">
      Search locations
      <input v-model="search" type="text" class="icon" placeholder="Search state" />

      <datalist id="states">
        <option v-for="youthChallenge in youthChallenges" v-bind:key="youthChallenge.state">
          {{ youthChallenge.state }}
        </option>
      </datalist>
      <div v-for="youthChallenge in filterSearch()" v-bind:key="youthChallenge.id">
        <h3 class="image">{{ youthChallenge.name }}</h3>
        <p>{{ youthChallenge.address }}</p>
        <a :href="youthChallenge.website">Location</a>
      </div>
    </div>
    <div class="map">
      <div id="map" style="height: 1000px"></div>
    </div>
  </div>
</template>
<style>
.icon {
  padding-left: 25px;
  background: url("/Users/keoshayoung/Desktop/Actualize/capstone-frontend/src/assets/search.svg") no-repeat left;
  background-size: 15px;
}
.image {
  padding-left: 25px;
  background: url("/Users/keoshayoung/Desktop/Actualize/capstone-frontend/src/assets/marker.svg") no-repeat left;
  background-size: 15px;
}
.container {
  display: grid;
  grid-template-columns: 1fr 2.2fr 0.1fr;
  grid-template-rows: 0.3fr 2.3fr 0.4fr;
  gap: 0px 0px;
  grid-auto-flow: row;
  grid-template-areas:
    ". . ."
    "searchbar map ."
    ". . .";
  width: 350px;
  height: 1000px;
}

#map {
  grid-area: map;
  position: absolute;
  top: 10%;
  bottom: 0;
  width: 65vw;
}

.searchbar {
  grid-area: searchbar;
}
</style>
