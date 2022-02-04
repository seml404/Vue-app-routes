<template>
  <div class="app">
    <div class="content">
      <div class="wrapper">
        <div class="nav-menu-wrapper">
          <nav-menu />
        </div>
        <div class="route-cards-wrapper">
          <button @click="showInfo">show info</button>
          <route-card
            v-for="(flight, idx) in flightsInfo"
            :key="idx"
            :flightData="flight.flight"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import NavMenu from "./components/NavMenu.vue";
import * as dataReceived from "./assets/flights.json";
import RouteCard from "./components/RouteCard.vue";
export default {
  name: "App",
  setup() {
    const flightsInfo = ref(dataReceived.default.result.flights);
    function showInfo() {
      console.log(flightsInfo.value);
    }
    return { flightsInfo, showInfo };
  },
  // data() {
  //   return {
  //     flightsInfo: ,
  //   };
  // },
  // methods: {
  //   showInfo() {
  //     console.log(this.flightsInfo);
  //   },
  // },
  components: { RouteCard, NavMenu },
};
</script>

<style>
@import "./styles.css";
.route-cards-wrapper {
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
