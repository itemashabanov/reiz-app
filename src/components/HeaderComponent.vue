<template>
  <header>
    <div>
      <ReizInput
        placeholder="Find something"
        :value="searchString"
        @input="$emit('search', $event.target.value)"
      />
    </div>
    <section class="flex between">
      <div class="flex">
        <ReizBtn
          text="Ascending"
          @click="changeSort('asc')"
          :class="{'reiz-btn--active': sortParam === 'asc'}"
        />
        <ReizBtn
          text="Descending"
          @click="changeSort('desc')"
          :class="{'reiz-btn--active': sortParam === 'desc'}"
        />
      </div>
      <div class="flex">
        <ReizBtn
          text="More than Lithuania"
          @click="filterByParam({
            'type': 'area',
            'value': 'Lithuania',
          })"
          :class="{'reiz-btn--active': filterType === 'area'}"
        />
        <ReizBtn
          text="In Oceania region"
          @click="filterByParam({
            'type': 'region',
            'value': 'Oceania',
          })"
          :class="{'reiz-btn--active': filterType === 'region'}"
        />
      </div>
    </section>
  </header>
</template>

<script>
import ReizInput from './ReizInput.vue'
import ReizBtn from './ReizBtn.vue'

export default {
  name: 'HeaderComponent',

  props: {
    searchString: String,
    sortParam: String,
    filterType: String,
  },

  data() {
    return {
      filter: {
        search: '',
        bySize: false,
        byRegion: false,
      },
    };
  },

  components: {
    ReizInput,
    ReizBtn,
  },

  methods: {
    changeSort(sortType) {
      this.$emit('sort', sortType);
    },

    filterByParam(filterParam) {
      if(this.filterType !== filterParam.type)
        this.$emit('filter', filterParam);
      else
        this.$emit('filter', {});
    }
  },
}
</script>

<style>
</style>
