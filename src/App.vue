<template>
  <div id="app">
    <img src="./assets/images/vue-weather.svg">
    <button @click="click()">test</button>
  </div>
</template>

<script>

export default {
  name: 'app',
  mounted() {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(
        (position) => {
          console.log(position)
        }, (error) => {
          console.log(error)
        }
      )
    } else console.log('Your browser does not support me.')
  },
  methods: {
    click() {
      // api.openweathermap.org/data/2.5/weather?lat={lat}&lon={lon}
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
}

body {
  display: flex;
  justify-content: center;
}

#app {
  img {
    width: 200px;
  }
}
</style>