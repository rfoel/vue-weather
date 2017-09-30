<template>
  <div id="app" class="day">
    <div class="orbit">
      <div class="sun"></div>
      <div class="moon"></div>
    </div>
    <main>
      <a class="search"></a>
      <div class="box">
        <h1>{{weather.name}}</h1>
        <h1 v-if="weather.main">{{weather.main.temp | temperature}}</h1>
        <h2 v-if="weather.main">Min: {{weather.main.temp_min | temperature}} Max: {{weather.main.temp_max | temperature}}</h2>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: "weather",
  data() {
    return {
      weather: {}
    }
  },
  mounted() {
    // AIzaSyAjmF9FpnCQ_-sA2ALIeIlCBj_4ljJzQW0 maps api
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(
        (position) => {
          console.log(position)
          this.getWeather(position)
        }, (error) => {
          console.log(error)
        }
      )
    } else console.log("Your browser does not support me.")
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

<style lang="scss" scoped>
#app {
  height: 100%;
  display: flex;
  flex-direction: column;
  align-content: center;
  justify-content: center;
  overflow: hidden;
}

h1,
h2 {
  text-align: center;
}

.night {
  background: #16222a;
  background: linear-gradient(to bottom, #16222a, #3a6073);
}

.day {
  background: #56ccf2;
  background: linear-gradient(to bottom, #56ccf2, #2f80ed);
}

.orbit {
  position: absolute;
  background: url("./../assets/images/header.svg") no-repeat center center;
  background-size: 60vh; // border: 1px solid #000; // margin-top: 8%; // border-radius: 50%;
  height: 60vh;
  width: 60vh;
  align-self: center;
  animation: spin-right 10s linear infinite;
}

.sun {
  background-color: #ffc107;
  border-radius: 50px;
  margin-left: -40px;
  margin-top: -40px;
  height: 80px;
  left: 50%;
  position: absolute;
  top: 0;
  width: 80px;
}

.moon {
  background: url("./../assets/images/moon.svg") no-repeat;
  background-size: 60px;
  margin-left: -40px;
  margin-bottom: 40px;
  height: 60px;
  left: 50%;
  position: absolute;
  bottom: 0;
  width: 60px;
}

@keyframes spin-right {
  100% {
    transform: rotate(360deg);
  }
}

main {
  position: absolute;
  background: #fff;
  bottom: 0;
  height: 50%;
  width: 100%;
  display: flex;
  justify-content: center;
  
  .box {
  align-self: center;

  }

  .search {
    display: block;
    background: url("./../assets/images/loupe.svg") no-repeat center;
    background-size: 40px;
    background-color: #fff;
    width: 50px;
    height: 50px;
    border-radius: 50%;
    padding: 10px;
    box-shadow: 0px 5px 20px 0px rgba(0, 0, 0, 0.3);
    position: absolute;
    top: 0;
    left: 50%;
    transform: translate(-50%, -50%);
    transition: all .1s ease-in-out;

    &:hover {
      top: -1%;
      background-color: #fafafa;
    }

    &:active,
    &:focus {
      background-color: #eee;
    }
  }
}
</style>