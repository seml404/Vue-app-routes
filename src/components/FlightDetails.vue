<template>
  <div class="route-card-leg">
    <div class="route-card-destinations">
      <p class="route-card-destination">
        {{ leg.segments[0]?.departureCity?.caption }},
        {{ leg.segments[0]?.departureAirport?.caption }}
      </p>
      <p class="route-card-destination-uid">
        ({{ leg.segments[0]?.departureAirport?.uid }})
      </p>
      <div class="route-card-destination-uid">&#8594;</div>
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
          }}
          ч
          {{
            departureDate.minute > 9
              ? departureDate.minute
              : `0` + departureDate.minute
          }}
          мин
        </p>
        <p class="route-card-date">
          {{ departureDateDay }}
        </p>
      </div>
      <div class="route-card-duration">
        <div class="route-card-clock-img">
          <img :src="clockImg" alt="" />
        </div>
        <p>{{ flightDurationHours }} ч {{ flightDurationMinutes }} - мин.</p>
      </div>
      <div class="route-card-time-info route-card-time-info-arrival">
        {{ arrivalDate.hour > 9 ? arrivalDate.hour : `0` + arrivalDate.hour }} ч
        {{
          arrivalDate.minute > 9 ? arrivalDate.minute : `0` + arrivalDate.minute
        }}
        мин
        <p class="route-card-date">{{ arrivalDateDay }}</p>
      </div>
    </div>
    <div class="route-card-changes">
      {{ changes }} пересад{{ changesEnding }}
    </div>
    <p class="route-card-transporter">
      Рейс выполняет:{{ leg.segments[0]?.airline?.caption }}
    </p>
  </div>
</template>

<script>
import { dateObj, localDateObj } from "../luxon";
import clock from "../assets/clock.png";
export default {
  props: ["leg"],
  data() {
    return {
      clockImg: clock,
      localParam: localDateObj.toLocaleString(
        this.leg.segments[0]?.departureDate
      ),
      departureDate: dateObj.fromISO(this.leg.segments[0]?.departureDate),
      arrivalDate: dateObj.fromISO(
        this.leg.segments[this.leg.segments?.length - 1]?.arrivalDate
      ),
    };
  },
  computed: {
    departureDateDay() {
      return this.departureDate
        .toLocaleString(dateObj.DATE_MED_WITH_WEEKDAY)
        .slice(0, -8);
    },
    arrivalDateDay() {
      return this.arrivalDate
        .toLocaleString(dateObj.DATE_MED_WITH_WEEKDAY)
        .slice(0, -8);
    },
    flightDuration() {
      return this.leg.duration;
    },
    flightDurationHours() {
      return Math.trunc(this.flightDuration / 60);
    },
    flightDurationMinutes() {
      return this.flightDuration % 60 === 0
        ? 0
        : this.flightDuration - this.flightDurationHours * 60;
    },
    changes() {
      return (this.leg.segments.length - 1).toString();
    },
    changesEnding() {
      let ending;
      if (this.changes[this.changes.length - 1] === "1") {
        ending = "ка";
      } else if (
        this.changes[this.changes.length - 1] === "2" ||
        this.changes[this.changes.length - 1] === "3"
      ) {
        ending = "ки";
      } else {
        ending = "ок";
      }
      return ending;
    },
  },
  methods: {},
};
</script>

<style lang="scss" scoped></style>
