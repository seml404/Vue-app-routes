<template>
  <div class="route-card-leg">
    <button @click="showDate">show date</button>
    <div class="route-card-destinations">
      <p class="route-card-destination">
        {{ leg.segments[0]?.departureCity?.caption }},
        {{ leg.segments[0]?.departureAirport?.caption }}
      </p>
      <p class="route-card-destination-uid">
        ({{ leg.segments[0]?.departureAirport?.uid }})
      </p>
      <div>arrow</div>
      <p class="route-card-destination">
        {{ leg.segments[leg.segments?.length - 1]?.arrivalCity?.caption }}
        {{ leg.segments[leg.segments?.length - 1]?.arrivalAirport?.caption }}
      </p>
      <p class="route-card-destination-uid">
        ({{ leg.segments[leg.segments?.length - 1]?.arrivalAirport?.uid }})
      </p>
    </div>
    <div class="route-card-timing">
      <div class="route-card-time-info route-card-time-info-departure">
        <p class="route-card-time">
          {{
            departureDate.hour > 9
              ? departureDate.hour
              : `0` + departureDate.hour
          }}ч{{
            departureDate.minute > 9
              ? departureDate.minute
              : `0` + departureDate.minute
          }}мин
        </p>
        <p class="route-card-date">
          {{ departureDateDay }}
        </p>
      </div>
      <div class="route-card-duration">
        <img src="#" alt="clock" class="clock-img" />
      </div>
      <div class="route-card-time-info route-card-time-info-arrival">
        {{
          arrivalDate.hour > 9 ? arrivalDate.hour : `0` + arrivalDate.hour
        }}ч{{
          arrivalDate.minute > 9
            ? arrivalDate.minute
            : `0` + arrivalDate.minute
        }}мин
        <p class="route-card-date">{{}}</p>
      </div>
      <div class="route-card-exchanges">{{}} пересадка</div>
      <p class="route-card-transporter">Рейс выполняет:{{}}</p>
    </div>
  </div>
</template>

<script>
import { dateObj, localDateObj } from "../luxon";
export default {
  props: ["leg"],
  data() {
    return {
      localParam: localDateObj.toLocaleString(
        this.leg.segments[0]?.departureDate
      ),
      departureDate: dateObj.fromISO(this.leg.segments[0]?.departureDate),
      arrivalDate: dateObj.fromISO(
        this.leg.segments[this.leg.segments?.length - 1]?.arrivalDate
      ),
      departureDateDay: "",
    };
  },
  mounted() {
    this.departureDateDay = this.departureDate
      .toLocaleString(dateObj.DATE_MED_WITH_WEEKDAY)
      .slice(0, -8);
  },
  methods: {
    showDate() {
      console.log();
    },
  },
};
</script>

<style lang="scss" scoped></style>
