<template>
  <div class="container-sm max-w-screen-lg mx-auto p-4">
    <h1 class="text-center text-3xl font-bold text-gray-800 mb-8">
      Country Information
    </h1>
    <div class="flex flex-col md:flex-row justify-center">
      <div class="md:w-1/2 p-4">
        <div class="bg-white rounded-lg p-4">
          <CountryDropdown
            :countries="countries"
            :defaultCountryId="defaultCountryId"
            @country-selected="fetchCountryDetails"
          />
          <Transition name="fade" mode="out-in">
            <CountryDetails
              v-if="selectedCountry"
              :key="selectedCountry.id"
              :country="selectedCountry"
            />
          </Transition>
        </div>
      </div>
      <div class="md:w-1/2 p-4">
        <div class="bg-white rounded-lg p-4">
          <AddCountryForm
            :continents="continents"
            @country-added="fetchCountries"
          />
        </div>
      </div>
    </div>
    <hr class="border-t border-slate-100 border-solid" />
    <div>
      <h2 class="text-xl font-bold mt-0 mb-4">List Of Countries</h2>
      <div class="flex flex-wrap justify-center sm:justify-start gap-6">
        <img
          @click="fetchCountryDetails(data.id)"
          v-for="data in countries"
          :key="data.id"
          :src="data.image"
          :alt="data.name"
          class="size-16 cursor-pointer rounded-full object-cover border-solid border-black shadow-lg shadow-slate-600"
        />
      </div>
    </div>
  </div>
</template>

<script>
import CountryDropdown from './components/CountryDropdown.vue';
import CountryDetails from './components/CountryDetails.vue';
import AddCountryForm from './components/AddCountryForm.vue';

export default {
  components: {
    CountryDropdown,
    CountryDetails,
    AddCountryForm,
  },
  data() {
    return {
      countries: [],
      selectedCountry: null,
      defaultCountryId: null,
    };
  },
  computed: {
    continents() {
      return this.countries.map((country) => country.continent);
    },
  },
  async mounted() {
    await this.fetchCountries();
    if (this.countries.length > 0) {
      this.defaultCountryId = this.countries[0].id;
      this.fetchCountryDetails(this.defaultCountryId);
    }
  },
  methods: {
    async fetchCountries() {
      try {
        const response = await fetch('http://localhost:8080/countries');
        this.countries = await response.json();
      } catch (error) {
        console.error('Error fetching countries:', error);
      }
    },
    async fetchCountryDetails(countryId) {
      try {
        const response = await fetch(
          `http://localhost:8080/country/${countryId}`
        );
        this.selectedCountry = await response.json();
      } catch (error) {
        console.error('Error fetching country details:', error);
      }
    },
  },
};
</script>
<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
