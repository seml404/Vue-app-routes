<template>
  <div class="route-card">
    <div class="route-card-top">
      <div><img alt="logo" /></div>
      <div class="route-card-info">
        <p class="route-card-price">
          {{ this.flightData.price.totalFeeAndTaxes.amount }}
        </p>
        <p>Стоимость для одного взрослого пассажира</p>
      </div>
    </div>

    <flight-details
      class="route-card-leg"
      v-for="leg in this.flightData.legs"
      :key="leg"
      :leg="leg"
    >
    </flight-details>
    <button class="btn btn-choose">show flight details</button>
    <button @click="handleClick">show flight details</button>
    <button @click="handleClick2">show more flight details</button>
  </div>
</template>

<script>
import FlightDetails from "./FlightDetails.vue";
export default {
  components: { FlightDetails },
  props: ["flightData"],
  methods: {
    handleClick() {
      console.log(this.flightData);
    },
    handleClick2() {
      console.log(this.flightData.legs);
    },
  },
};
</script>

<style scoped></style>
