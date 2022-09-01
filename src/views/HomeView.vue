<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      youthChallenges: [],
    };
  },
  created: function () {
    this.indexYouthChallenges();
  },
  methods: {
    indexYouthChallenges: function () {
      axios.get("http://localhost:3000/youth_challenges.json").then((response) => {
        this.youthChallenges = response.data;
        console.log("all Youth Challenges:", this.youthChallenges);
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h2>All Youth Challenges</h2>
    <div v-for="youthChallenge in youthChallenges" v-bind:key="youthChallenge.id">
      <h4>{{ youthChallenge.name }}</h4>
      <p>{{ youthChallenge.address }}</p>
    </div>
  </div>
</template>

<style></style>
