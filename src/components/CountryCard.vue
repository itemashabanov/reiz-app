<template>
  <article class="country-card">
    <h2 class="country-card__title">{{ country.name }}</h2>
    <p>Region: {{ country.region }}</p>
    <p>Area size: {{ country.area }} km<sup>2</sup></p>
  </article>
</template>

<script>

export default {
  name: 'CountryCard',

  props: {
    country: Object,
  },
}
</script>

<style>
  .country-card {
    border: 1px solid teal;
    border-radius: 4px;
    background-color: #e6e6e6;
    padding: 12px 24px;
    margin-top: 24px;
  }
  .country-card__title {
    margin-bottom: 12px;
  }
</style>
