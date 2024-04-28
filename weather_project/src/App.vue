<template>
  <main>
    <!-- Date -->
    <div class="text-center mb-6">
      {{
        new Date().toLocaleDateString('en-us', {
          weekday: 'long',
          year: 'numeric',
          month: 'long',
          day: 'numeric'
        })
      }}
    </div>

    <!-- Search -->
    <div>
      <SearchInput @place-data="addPlace"></SearchInput>
    </div>

    <!-- Weather Card -->
    <div class="grid grid-cols-2 gap-4">
      <div v-for="(place, index) in places" :key="index">
        <WeatherCard :place="place" @DeletePlace="deletePlace" />
      </div>
    </div>
  </main>
</template>

<script setup>
import SearchInput from './components/SearchInput.vue'
import { ref } from 'vue'
import WeatherCard from './components/WeatherCard.vue'

const places = ref([])

const addPlace = (data) => {
  places.value.push(data)
}

const deletePlace = (name) => {
  if(comfirm('Are you Sure'))
  {
    place.value=place.value.filters(p => p.location.name !== name)
  }
}
</script>
