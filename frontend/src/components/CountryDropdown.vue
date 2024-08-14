<template>
  <div class="relative inline-block w-64 mb-5">
    <select
      v-model="selectedCountryId"
      @change="selectCountry"
      class="block appearance-none w-full bg-white border border-gray-300 hover:border-gray-400 px-4 py-2 pr-8 cursor-pointer rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent"
    >
      <option value="" disabled>Select a Country</option>
      <option
        v-for="country in countries"
        :key="country.id"
        :value="country.id"
      >
        {{ country.name }}
      </option>
    </select>
  </div>
</template>

<script>
export default {
  props: ['countries'],
  data() {
    return {
      selectedCountryId: '',
    };
  },
  methods: {
    selectCountry() {
      this.$emit('country-selected', this.selectedCountryId);
    },
  },
};
</script>
