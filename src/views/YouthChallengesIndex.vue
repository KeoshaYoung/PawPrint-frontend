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
    this.mapApp();
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
    mapApp: function () {
      mapboxgl.accessToken = process.env.VUE_APP_MY_API_KEY;
      const map = new mapboxgl.Map({
        container: "map", // container ID
        style: "mapbox://styles/kyoung1012/cl7tt1ucf000114mohi2ld85x", // style URL
        center: [-84.388, 33.749], // starting position [lng, lat]
        zoom: 9, // starting zoom
        projection: "globe", // display the map as a 3D globe
      });

      map.on("style.load", () => {
        map.setFog({}); // Set the default atmosphere style
      });
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
  grid-template-columns: 1.5fr 0.1fr 1.7fr;
  grid-template-rows: 0.2fr 1.8fr 1fr;
  gap: 5px 6px;
  grid-template-areas:
    ". . ."
    "SearchBar SearchBar map"
    ". . .";
  width: 50%;
  height: 50%;
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
