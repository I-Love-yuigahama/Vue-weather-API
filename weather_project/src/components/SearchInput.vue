<template>
  <div>
    <!-- search field -->
    <form>
      <div class="bg-white border border-indigo-600/30 rounded-lg shadow-lg flex items-center">
        <i class="fa-solid fa-magnifying-glass p-2 text-indigo-600"></i>
        <input
          type="text"
          placeholder="Search for a place"
          class="rounded-r-lg p-2 border-0 outline-0 focus:ring-2 focus:ring-indigo-600 ring-inset w-full"
          v-model="seachTerm.query"
          @input="handleSearch"
        />
      </div>
    </form>
    <!-- search suggestions -->
    <div class="bg-white my-2 rounded-lg shadow-lg">
      <div v-if="seachTerm.result !== null">
        <div v-for="place in seachTerm.result" :key="place.id">
          <button
            @click.prevent="getWeather(place.id)"
            class="px-3 my-2 hover:text-indigo-600 hover:font-bold w-full text-left"
          >
            {{ place.name }} , {{ place.region }} , {{ place.country }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive } from 'vue'

const emit = defineEmits(['place-data'])

const seachTerm = reactive({
  query: '',
  timeout: null,
  result: null
})

const handleSearch = () => {
  clearTimeout(seachTerm.timeout)

  seachTerm.timeout = setTimeout(async () => {
    if (seachTerm.query !== '') {
      const Respone = await fetch(`
                    http://api.weatherapi.com/v1/search.json?key=498af09c81ae42d091125726240404&q=${seachTerm.query}
                    `)

      const data = await Respone.json()
      seachTerm.result = data
    } else {
      seachTerm.result = null
    }
  }, 500)
}

const getWeather = async (id) => {
  const res = await fetch(
    `http://api.weatherapi.com/v1/forecast.json?key=498af09c81ae42d091125726240404&q=id:${id}&days=3&aqi=no&alerts=no`
  )
  const data = await res.json()
  emit('place-data' ,data )

  seachTerm.query = ''
  seachTerm.result = null
}
</script>
