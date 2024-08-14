<template>
  <div class="bg-white rounded pt-0 pb-6 mb-4">
    <h2 class="text-xl font-bold mt-0 mb-4">Add New Country</h2>
    <form @submit.prevent="submitForm">
      <div class="mb-4">
        <label class="block text-gray-500 text-sm font-bold mb-2" for="name">
          Country Name
        </label>
        <input
          v-model="name"
          type="text"
          id="name"
          class="border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none"
          :class="{ 'border-red-500': errors.name }"
        />
        <p v-if="errors.name" class="text-red-500 text-xs bold">
          {{ errors.name }}
        </p>
      </div>
      <div class="mb-4">
        <label
          class="block text-gray-500 text-sm font-bold mb-2"
          for="continent"
        >
          Continent
        </label>
        <select
          v-model="continent"
          id="continent"
          class="border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none"
        >
          <option value="" disabled>Select a Continent</option>
          <option
            v-for="continent in uniqueContinents"
            :key="continent"
            :value="continent"
          >
            {{ continent }}
          </option>
        </select>
      </div>
      <div class="mb-4">
        <label class="block text-gray-500 text-sm font-bold mb-2" for="rank">
          Rank
        </label>
        <input
          v-model="rank"
          type="number"
          id="rank"
          class="border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none"
          :class="{ 'border-red-500': errors.rank }"
        />
        <p v-if="errors.rank" class="text-red-500 text-xs bold">
          {{ errors.rank }}
        </p>
      </div>
      <div class="mb-4">
        <label class="block text-gray-500 text-sm font-bold mb-2" for="image">
          Country Image
        </label>
        <input
          @change="handleFileUpload"
          type="file"
          id="image"
          accept=".jpg,.jpeg,.png"
          class="border rounded w-full py-2 text-gray-700 leading-tight focus:outline-none"
          :class="{ 'border-red-500': errors.image }"
        />
        <p v-if="errors.image" class="text-red-500 text-xs bold">
          {{ errors.image }}
        </p>
      </div>
      <div class="flex items-center justify-between">
        <button
          type="submit"
          class="w-full bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 cursor-pointer rounded focus:outline-none"
        >
          Add Country
        </button>
      </div>
      <div
        v-if="successMessage"
        class="bg-teal-100 border-t-4 border-teal-500 rounded-b text-teal-900 px-1 py-1 mt-2 shadow-md text-center"
        role="alert"
      >
        <div>
          <p class="font-bold">Country added successfully</p>
        </div>
      </div>
      <div
        v-if="errorMessage"
        class="bg-red-100 border-t-4 border-red-400 rounded-b text-red-700 px-1 py-1 mt-2 shadow-md text-center"
        role="alert"
      >
        <div>
          <p class="font-bold">Problem occured while adding country</p>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  props: {
    continents: Array,
  },
  data() {
    return {
      name: '',
      continent: '',
      rank: null,
      image: null,
      errors: {},
      errorMessage: false,
      successMessage: false,
    };
  },
  computed: {
    uniqueContinents() {
      return [...new Set(this.continents)];
    },
  },
  methods: {
    handleFileUpload(event) {
      this.image = event.target.files[0];
    },
    validateForm() {
      this.errors = {};
      if (!this.name || this.name.length < 3 || this.name.length > 20) {
        this.errors.name = 'Country name must be between 3 and 20 characters.';
      }
      if (!this.rank || isNaN(this.rank)) {
        this.errors.rank = 'Rank must be a numeric value.';
      }
      if (
        !this.image ||
        !['image/jpeg', 'image/png'].includes(this.image.type) ||
        this.image.size > 4 * 1024 * 1024
      ) {
        this.errors.image = 'Image must be a JPG/PNG file and less than 4 MB.';
      }
      return Object.keys(this.errors).length === 0;
    },
    async submitForm() {
      if (this.validateForm()) {
        const formData = new FormData();
        formData.append('name', this.name);
        formData.append('continent', this.continent);
        formData.append('rank', this.rank);
        formData.append('image', this.image);

        try {
          const response = await fetch('http://localhost:8080/country', {
            method: 'POST',
            body: formData,
          });
          if (response.ok) {
            this.$emit('country-added');
            this.name = '';
            this.continent = '';
            this.rank = null;
            this.image = null;
            this.successMessage = true;
            setTimeout(() => {
              this.successMessage = false;
            }, 2000);
          } else {
            const data = await response.json();
            this.errors = data.errors || {};
          }
        } catch (error) {
          console.error('Error adding country:', error);
        }
      }
    },
  },
};
</script>
