<script setup>
import { ref } from 'vue'

defineProps({
  msg: String
})

const count = ref(0)
</script>

<template>
  <h1>Weather API ☁️</h1>
  <input type="text" placeholder="Введите город.." v-on:keyup.enter="getWeather" v-model="cityInput" autocomplete="off">
  <div class="info-container" v-if="seenInfo">
    <h3>{{ currentCity }}</h3>
    <p>Сейчас: {{ temp }}℃</p>
    <p>Ощущается как: {{ feelsTemp}}℃</p>
  </div>
</template>

<style scoped>
a {
  color: #42b983;
}
</style>

<script>
  export default{
    name: "Weather",
    data() {
      return {
        key:"0dfa22e7d70560f1dbbceb6280217962",
        seenInfo: false,
        cityInput: "Moscow",
        currentCity: "Moscow",
        temp: 100,
        feelsTemp: 10
      }
    },
    methods: {
      async getWeather() {
        this.seenInfo = true;
        const baseURL = `https://api.openweathermap.org/data/2.5/weather?q=${this.cityInput}&appid=${this.key}&units=metric`;
        try {
          const response = await fetch(baseURL);
          const data = await response.json();
          this.currentCity = data.name;
          this.temp = Math.round(data.main.temp);
          this.feelsTemp = Math.round(data.main.feels_like);
        } catch (error) {
          console.log(error);
        }
      }
    }
  }
</script>