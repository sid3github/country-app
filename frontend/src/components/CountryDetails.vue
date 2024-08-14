<template>
  <div v-if="country" class="max-w-sm rounded overflow-hidden bg-white">
    <div class="relative">
      <img
        class="w-full h-100 object-cover"
        :src="country.image"
        :alt="country.name"
      />
      <div
        class="absolute top-0 right-0 bg-blue-500 text-white text-xs font-bold rounded-bl-lg px-2 py-1"
      >
        Rank #{{ country.rank }}
      </div>
    </div>
    <div class="py-4">
      <div class="text-gray-700 text-xl mb-2">
        <b>Country Name:</b> {{ country.name }}
      </div>
      <p class="text-gray-700 text-base my-2">
        <b>Continent:</b> {{ country.continent }}
      </p>
    </div>
    <div>
      <span
        class="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2"
      >
        #{{ country.name.toLowerCase().replace(' ', '') }}
      </span>
      <span
        class="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2"
      >
        #{{ country.continent.toLowerCase() }}
      </span>
    </div>
  </div>
</template>

<script>
export default {
  props: ['country'],
};
</script>
