<template>
  <div class="nav-menu">
    <div class="nav-menu-section">
      <p class="nav-menu-title">Сортировать</p>
      <div class="inputs-wrapper">
        <div class="input-wrapper">
          <input
            type="radio"
            name="filter-radio-btn"
            v-model="sorting"
            value="priceIncrease"
            @change="handleSetSorting"
          />
          - по возрастанию цены
        </div>
        <div class="input-wrapper">
          <input
            type="radio"
            name="filter-radio-btn"
            v-model="sorting"
            value="priceDecrease"
            @change="handleSetSorting"
          />
          - по убыванию цены
        </div>
        <div class="input-wrapper">
          <input
            type="radio"
            name="filter-radio-btn"
            v-model="sorting"
            value="travelTime"
            @change="handleSetSorting"
          />
          - по времени в пути
        </div>
      </div>
    </div>
    <div class="nav-menu-section">
      <p class="nav-menu-title">Фильтровать</p>
      <div class="inputs-wrapper">
        <div class="input-wrapper">
          <input
            type="checkbox"
            value="oneEchange"
            v-model="filter.oneExchange"
            @change="handleSetFilter"
          />
          - 1 пересадка
        </div>
        <div class="input-wrapper">
          <input
            type="checkbox"
            value="noEchanges"
            v-model="filter.noEchanges"
            @change="handleSetFilter"
          />
          - без пересадок
        </div>
      </div>
    </div>
    <div class="nav-menu-section">
      <p class="nav-menu-title">Цена</p>
      <div class="inputs-wrapper">
        <div class="input-wrapper">
          <p>От</p>
          <input type="number" v-model="price.from" @change="handleSetPrice" />
        </div>
        <div class="input-wrapper">
          <p>До</p>
          <input type="number" v-model="price.till" @change="handleSetPrice" />
        </div>
      </div>
    </div>
    <div class="nav-menu-section">
      <p class="nav-menu-title">Авиакомпании</p>
      <div class="inputs-wrapper">
        <div
          class="input-wrapper"
          v-for="company in airCompanies"
          :key="company"
        >
          <input
            type="checkbox"
            @change="handleSetAirCompany"
            :value="company"
          />
          - {{ company }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      sorting: "",
      filter: { noEchanges: false, oneExchange: false },
      price: { from: null, till: null },
      airCompany: [],
    };
  },
  methods: {
    handleSetSorting() {
      this.$emit("handleSetSorting", this.sorting);
    },
    handleSetFilter() {
      this.$emit("handleSetFilter", this.filter);
    },
    handleSetPrice() {
      this.$emit("handleSetPrice", this.price);
    },
    handleSetAirCompany(e) {
      this.airCompany.includes(e.target.value)
        ? (this.airCompany = this.airCompany.filter(
            (item) => item !== e.target.value
          ))
        : this.airCompany.push(e.target.value);
      this.$emit("handleSetAirCompany", this.airCompany);
    },
  },
  props: ["airCompanies"],
};
</script>

<style lang="scss" scoped></style>
