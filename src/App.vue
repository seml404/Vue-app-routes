<template>
  <div class="app">
    <div class="content">
      <div class="wrapper">
        <div class="nav-menu-wrapper">
          <nav-menu
            :airCompanies="listOfAirCompanies"
            @handleSetSorting="setSorting"
            @handleSetFilter="setFilter"
            @handleSetPrice="setPrice"
          />
        </div>
        <div class="route-cards-wrapper">
          <button @click="showInfo">show info</button>
          <button @click="showInfoAir">show air comp</button>
          <button @click="filterChanges">filter</button>
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
import NavMenu from "./components/NavMenu.vue";
import * as dataReceived from "./assets/flights.json";
import RouteCard from "./components/RouteCard.vue";
export default {
  name: "App",
  computed: {
    listOfAirCompanies() {
      const list = [];
      this.flightsInfo.forEach((item) => {
        if (!list.includes(item.flight.carrier.caption)) {
          list.push(item.flight.carrier.caption);
        }
      });
      return list.sort((val1, val2) => {
        return val1[0] < val2[0] ? -1 : 1;
      });
    },
  },
  data() {
    return {
      flightsInfo: dataReceived.default.result.flights,
      sorting: null,
      filter: { noEchanges: false, oneExchange: false },
      price: { from: null, till: null },
    };
  },
  methods: {
    setSorting(value) {
      this.sorting = value;
    },
    setFilter(value) {
      this.filter = value;
    },
    setPrice(value) {
      this.price = value;
    },
    sortItems(arr, criteria) {
      switch (criteria) {
        case "priceIncrease":
          return arr.sort((item1, item2) => {
            item1.flight.price.totalFeeAndTaxes.amount <
            item2.flight.price.totalFeeAndTaxes.amount
              ? -1
              : 1;
          });
        case "priceDecrease":
          return arr.sort((item1, item2) => {
            item1.flight.price.totalFeeAndTaxes.amount >
            item2.flight.price.totalFeeAndTaxes.amount
              ? -1
              : 1;
          });
        case "travelTime": {
          return arr.sort((item1, item2) => {
            item1.flight.legs.duration > item2.flight.legs.duration ? -1 : 1;
          });
        }
      }
    },
    filterChanges(item, value) {
      let checkRes = [];
      item.flight.legs.forEach((leg) => {
        if (leg.segments.length - 1 === value) {
          checkRes.push(true);
        } else {
          checkRes.push(false);
        }
      });
      console.log(checkRes);
      return !checkRes.includes(false);
    },
    filterPrice(arr, startPrice, endPrice) {
      if (startPrice) {
        arr = arr.filter((item) => {
          item.flight.price.totalFeeAndTaxes > startPrice;
        });
      }
      if (endPrice) {
        arr = arr.filter((item) => {
          item.flight.price.totalFeeAndTaxes < endPrice;
        });
        return arr;
      }
    },

    // filterChanges() {
    //   function checkFlights(item) {
    //     let checkRes = [];
    //     item.flight.legs.forEach((leg) => {
    //       if (leg.segments.length - 1 === 1) {
    //         checkRes.push(true);
    //       } else {
    //         checkRes.push(false);
    //       }
    //     });
    //     console.log(checkRes);
    //     return !checkRes.includes(false);
    //   }
    //   this.flightsInfo = this.flightsInfo.filter((item) => checkFlights(item));
    // },
    showInfo() {
      console.log(this.flightsInfo);
    },
    showInfoAir() {
      console.log(this.listOfAirCompanies);
    },
  },
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
