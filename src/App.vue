<template>
  <section class="main">
    <HeaderComponent
      :searchString="searchString"
      @search="searchString = $event"
      :sortParam="sortParam"
      @sort="sortParam = $event"
      :filterType="filterParam.type"
      @filter="filterParam = $event"
    />

    <template v-for="country in paginatedData" :key="country.name">
      <CountryCard :country="country" />
    </template>

    <section class="reiz-pagination">
      <div>
        <button
          class="reiz-pagination__btn"
          @click="prevPage"
          :disabled="pageNumber === 0"
        >
          Previous
        </button>
        <button
          class="reiz-pagination__btn"
          @click="nextPage"
          :disabled="pageNumber >= pageCount - 1"
        >
          Next
        </button>
      </div>
      <p>{{ pageNumber + 1 }} from {{ pageCount }}</p>
    </section>

    <div v-if="!countries" class="error-block flex">
      <p>Nothing is here</p>
    </div>
  </section>
</template>

<script>
import HeaderComponent from './components/HeaderComponent.vue'
import CountryCard from './components/CountryCard.vue'

export default {
  name: 'App',

  components: {
    HeaderComponent,
    CountryCard
  },

  data() {
    return {
      countries: null,
      searchString: '',
      sortParam: '',
      filterParam: {},
      pageNumber: 0,
      itemsPerPage: 10,
    };
  },

  watch: {
    sortParam() {
      this.countries.sort(this.sortingByField('name', this.sortParam));
    },
  },

  computed: {
    filteredCountries() {
      if (!this.countries) return null;

      const searchFilter = (country) =>
        [country.name, country.area, country.region]
          .join(' ')
          .toLowerCase()
          .includes(this.searchString.toLowerCase());

      let newCountries = [...this.countries];
      
      if(this.filterParam.type === 'region') {
        const countryRegion = this.filterParam.value;
      
        newCountries = this.countries.filter(
          country => country.region === countryRegion,
        );
      }

      if(this.filterParam.type === 'area') {
        const  areaSize = this.countries.find(country => country.name === this.filterParam.value).area;
      
        newCountries = this.countries.filter(
          country => country.area > areaSize,
        );
      }

      return newCountries.filter(
        (country) => 
          searchFilter(country),
      );
    },

    pageCount() {
      if(!this.filteredCountries) return null;

      return Math.ceil(this.filteredCountries.length / this.itemsPerPage);
    },

    paginatedData() {
      if(!this.filteredCountries) return null;

      const start = this.pageNumber * this.itemsPerPage,
            end = start + this.itemsPerPage;
      
      return this.filteredCountries.slice(start, end);
    }
  },

  methods: {
    sortingByField(field, sortType) {
      if(sortType === 'asc')
        return (a, b) => a[field] > b[field] ? 1 : -1;

      if(sortType === 'desc')
        return (a, b) => a[field] < b[field] ? 1 : -1;
    },

    nextPage(){
        this.pageNumber++;
    },
    prevPage(){
      this.pageNumber--;
    }
  },

  created() {
    fetch('https://restcountries.com/v2/all?fields=name,region,area')
      .then(res => res.json())
      .then(country => this.countries = country);
  },
}
</script>

<style>
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
    font-family: Arial;
  }
  .flex {
    display: flex;
  }
  .between {
    justify-content: space-between;
  }
  .main {
    max-width: 1300px;
    margin: 0 auto;
    padding: 24px;
  }
  .error-block {
    background-color: lightcoral;
    border-radius: 4px;
    justify-content: center;
    align-items: center;
    height: 100px;
    margin-top: 24px;
    text-transform: uppercase;
    font-size: 36px;
    color: #fff;
    font-weight: 700;
  }
  .reiz-pagination {
    margin-top: 24px;
  }
  .reiz-pagination__btn {
    border: 0;
    border-radius: 4px;
    padding: 6px 12px;
    font-size: 14px;
    background-color: cornflowerblue;
    color: #fff;
    margin-bottom: 12px;
    cursor: pointer;
  }
  .reiz-pagination__btn:hover {
    background-color: darkcyan;
  }
  .reiz-pagination__btn:first-child {
    margin-right: 14px;
  }
</style>
