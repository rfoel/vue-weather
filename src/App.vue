<template>
  <div id="app">
    <img src="./assets/images/vue-weather.svg">
    <h1>{{weather.name}}</h1>
    <h1 v-if="weather.main">{{weather.main.temp | temperature}}</h1>
    <h2 v-if="weather.main">Max: {{weather.main.temp_max | temperature}} Min: {{weather.main.temp_min | temperature}}</h2>
  </div>
</template>

<script>

export default {
  name: 'app',
  data() {
    return {
      weather: {}
    }
  },
  mounted() {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(
        (position) => {
          console.log(position)
          this.getWeather(position)
        }, (error) => {
          console.log(error)
        }
      )
    } else console.log('Your browser does not support me.')
  },
  methods: {
    getWeather(position) {
      let lat = position.coords.latitude
      let lon = position.coords.longitude
      this.$http.get(`http://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=a793d7777a5d7cf566cbd52f98c985c9&units=metric`).then(response => {
        console.log(response.body)
        this.weather = response.body
      }, error => {
        console.log(error)
      })
    }
  },
  filters: {
    temperature: (value) => {
      console.log(value)
      return `${value.toFixed(0)}°`
    }
  }
}
</script>

<style lang="scss">
html,
body {
  height: 100%;
  margin: 0;
  padding: 0;
  font-family: 'Source Sans Pro', sans-serif;
}

body {
  display: flex;
  justify-content: center;
}

img {
  display: block;
  width: 200px;
}

h1, h2 {
  text-align: center;
}

h1 {
  font-size: 6vw;
}
</style>