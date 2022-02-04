<template>
  <div class="nav-menu">
    <div class="nav-menu-section">
      <p class="nav-menu-title">Сортировать</p>
      <div class="inputs-wrapper">
        <div class="input-wrapper">
          <input type="radio" /> - по возрастанию цены
        </div>
        <div class="input-wrapper">
          <input type="radio" /> - по убыванию цены
        </div>
        <div class="input-wrapper">
          <input type="radio" /> - по времени в пути
        </div>
      </div>
    </div>
    <div class="nav-menu-section">
      <p class="nav-menu-title">Фильтровать</p>
      <div class="inputs-wrapper">
        <div class="input-wrapper"><input type="checkbox" /> - 1 пересадка</div>
        <div class="input-wrapper">
          <input type="checkbox" /> - без пересадок
        </div>
      </div>
    </div>
    <div class="nav-menu-section">
      <p class="nav-menu-title">Цена</p>
      <div class="inputs-wrapper">
        <div class="input-wrapper">
          <p>От</p>
          <input type="number" />
        </div>
        <div class="input-wrapper">
          <p>До</p>
          <input type="number" />
        </div>
      </div>
    </div>
    <div class="nav-menu-section">
      <p class="nav-menu-title">Авиакомпании</p>
      <div class="inputs-wrapper">
        <div class="input-wrapper"><input type="checkbox" /> - 1 пересадка</div>
        <div class="input-wrapper">
          <input type="checkbox" /> - без пересадок
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {};
</script>

<style lang="scss" scoped></style>
