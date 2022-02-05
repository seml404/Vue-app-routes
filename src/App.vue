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
            @handleSetAirCompany="setAirCompany"
          />
        </div>
        <div class="route-cards-wrapper" v-if="flightsDataToRender.length > 0">
          <route-card
            v-for="(flight, idx) in flightsDataToRender"
            :key="idx"
            :flightData="flight.flight"
          />
          <button @click="handlePaginate" class="btn btn-load">
            Показать еще
          </button>
        </div>
        <div class="route-cards-wrapper" v-else>
          <p>Рейсы по заданным условиям не обнаружены</p>
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
    // метод формирования перечня авиакомпания, для отражения в боковом меню
    listOfAirCompanies() {
      const list = [];
      this.flightsInfo.forEach((item) => {
        if (!list.includes(item.flight.carrier.caption)) {
          list.push(item.flight.carrier.caption);
        }
      });
      return list.sort((val1, val2) => {
        return val1 < val2 ? -1 : 1;
      });
    },
    // метод формирования перечня рейсов, вызывающий методы для их фильтрации/сортировки
    flightsData() {
      let amendedArr = this.flightsInfo;
      if (this.filter.noEchanges || this.filter.oneExchange) {
        let param1 = this.filter.noEchanges ? 0 : null;
        let param2 = this.filter.oneExchange ? 1 : null;
        amendedArr = amendedArr.filter((item) =>
          this.filterChanges(item, param1, param2)
        );
      }
      if (this.price.from || this.price.till) {
        amendedArr = this.filterPrice(
          amendedArr,
          this.price.from,
          this.price.till
        );
      }
      if (this.airCompany.length > 0) {
        amendedArr = this.filterAirCompany(amendedArr, this.airCompany);
      }
      if (this.sorting) {
        amendedArr = this.sortItems(amendedArr, this.sorting);
      }
      console.log(amendedArr.length);
      return amendedArr;
    },
    // методы для работы с пагинацией
    flightsDataProcessed() {
      return this.flightsData;
    },
    flightsDataToRender() {
      return this.flightsDataProcessed.slice(0, this.flightsDataPaginationIdx);
    },
  },
  data() {
    return {
      flightsInfo: dataReceived.default.result.flights,
      sorting: null,
      filter: { noEchanges: false, oneExchange: false },
      price: { from: null, till: null },
      airCompany: [],
      flightsDataPaginationIdx: 3,
    };
  },
  components: { RouteCard, NavMenu },
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
    setAirCompany(value) {
      this.airCompany = value;
    },
    // метод сортировки рейсов по возрастанию/убыванию цены и по времени в пути
    sortItems(arr, criteria) {
      switch (criteria) {
        case "priceIncrease":
          arr = arr.sort((item1, item2) => {
            return +item1.flight.price.totalFeeAndTaxes.amount <
              +item2.flight.price.totalFeeAndTaxes.amount
              ? -1
              : 1;
          });
          return arr;
        case "priceDecrease":
          arr = arr.sort((item1, item2) => {
            return +item1.flight.price.totalFeeAndTaxes.amount >
              +item2.flight.price.totalFeeAndTaxes.amount
              ? -1
              : 1;
          });
          return arr;
        case "travelTime":
          arr = arr.sort((item1, item2) => {
            return +item1.flight.legs[0].duration +
              +item1.flight.legs[1].duration <
              +item2.flight.legs[0].duration + +item2.flight.legs[1].duration
              ? -1
              : 1;
          });
          return arr;
      }
    },
    // метод фильтрации рейсов по авиаперевозчику
    filterAirCompany(arr, values) {
      this.flightsDataPaginationIdx = 3;
      arr = arr.filter((item) => {
        return values.includes(item.flight.carrier.caption);
      });
      return arr;
    },
    // метод фильтрации рейсов по пересадкам
    filterChanges(item, value1, value2) {
      this.flightsDataPaginationIdx = 3;
      let checkRes = [];
      item.flight.legs.forEach((leg) => {
        if (
          leg.segments.length - 1 === value1 ||
          leg.segments.length - 1 === value2
        ) {
          checkRes.push(true);
        } else {
          checkRes.push(false);
        }
      });
      return !checkRes.includes(false);
    },
    // метод фильтрации рейсов по цене
    filterPrice(arr, startPrice, endPrice) {
      this.flightsDataPaginationIdx = 3;
      if (startPrice) {
        arr = arr.filter((item) => {
          return +item.flight.price.totalFeeAndTaxes.amount > +startPrice;
        });
      }
      if (endPrice) {
        arr = arr.filter((item) => {
          return +item.flight.price.totalFeeAndTaxes.amount < +endPrice;
        });
      }
      return arr;
    },
    // обработчик нажатия кнопки для пагинации
    handlePaginate() {
      let idxToAdd;
      if (
        this.flightsDataPaginationIdx ===
        this.flightsDataProcessed.length - 1
      ) {
        return;
      }
      idxToAdd =
        this.flightsDataPaginationIdx + 3 < this.flightsDataProcessed.length - 1
          ? 3
          : this.flightsDataProcessed.length -
            1 -
            this.flightsDataPaginationIdx;
      this.flightsDataPaginationIdx += idxToAdd;
    },
  },
};
</script>

<style>
@import "./styles.css";
</style>
