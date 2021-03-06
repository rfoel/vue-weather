<template>
  <div id="app" :class="day ? 'day' : 'night'">
    <div class="orbit">
      <div class="sun"></div>
      <div class="moon"></div>
    </div>
    <main>
      <a class="search" @click="toggleSearch()"></a>
      <div class="search-box" v-if="isSearching" v-on-clickaway="toggleSearch">
        <label for="">Find a place!</label>
        <input type="text" @input="searchPlace()" v-model="place">
      </div>
      <div class="box">
        <div class="row">
          <div class="clock">
            <svg id="clock" width="100%" height="100%" viewBox="0 0 200 200">
              <circle cx="100" cy="100" r="80" stroke-width="12px" fill="#fff" stroke="#16222a" />
              <g stroke-linejoin="round" stroke-linecap="round" stroke="#16222a">
                <line y2="30" transform="rotate(0 100 100)" x2="100" y1="100" x1="100" stroke-width="2px" />
                <line y2="40" transform="rotate(0 100 100)" x2="100" y1="100" x1="100" stroke-width="4px" />
                <line y2="55" transform="rotate(80 100 100)" x2="100" y1="100" x1="100" stroke-width="3px" />
              </g>
            </svg>
          </div>
          <span class="time"> {{datetime | time}}</span>
        </div>
        <div class="row">
          <h1>{{weather.name}}</h1>
        </div>
        <div class="row">
          <h1 v-if="weather.main">{{weather.main.temp | temperature}}</h1>
        </div>
        <div class="row">
          <h2 v-if="weather.main">Min: {{weather.main.temp_min | temperature}} Max: {{weather.main.temp_max | temperature}}</h2>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import { mixin as clickaway } from "vue-clickaway"

let google

export default {
  name: "weather",
  mixins: [clickaway],
  data() {
    return {
      weather: {},
      datetime: new Date(),
      day: true,
      minuteOfDay: 0,
      isSearching: false,
      place: ""
    }
  },
  mounted() {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(
        (position) => {
          this.getWeather(position)
          this.startClock()
        }, (error) => {
          console.log(error)
        }
      )
    } else console.log("Your browser does not support me.")

    const autocomplete = new google.maps.places.Autocomplete(this.place)
    console.log(autocomplete)
  },
  methods: {
    getWeather(position) {
      let lat = position.coords.latitude
      let lon = position.coords.longitude
      this.$http.get(`http://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=a793d7777a5d7cf566cbd52f98c985c9&units=metric`).then(response => {
        this.weather = response.body
        console.log(response.body)
      }, error => {
        console.log(error)
      })
    },
    startClock() {
      setInterval(() => this.updateClock(), 1000)
      // this.updateClock()
    },
    updateClock() {
      const hands = document.querySelectorAll("#clock line")

      const cx = 100
      const cy = 100
      this.datetime = new Date()
      // this.datetime = new Date(this.datetime.getTime() + this.minuteOfDay * 60000)

      let hoursAngle = 360 * this.datetime.getHours() / 12 + this.datetime.getMinutes() / 2
      let minutesAngle = 360 * this.datetime.getMinutes() / 60
      let secondsAngle = 360 * this.datetime.getSeconds() / 60
      hands[0].setAttribute("transform", `rotate(${[secondsAngle, cx, cy].join(" ")})`)
      hands[1].setAttribute("transform", `rotate(${[minutesAngle, cx, cy].join(" ")})`)
      hands[2].setAttribute("transform", `rotate(${[hoursAngle, cx, cy].join(" ")})`)

      this.day = this.datetime.getHours() >= 6 && this.datetime.getHours() < 18
      this.orbit()
    },
    orbit() {
      const orbit = document.querySelector(".orbit")
      let orbitAngle = ((this.datetime.getMinutes()) + (60 * this.datetime.getHours())) / 4
      orbit.style.transform = `rotate(${orbitAngle}deg)`
    },
    toggleSearch() {
      this.isSearching = !this.isSearching
    },
    searchPlace() {
      if (this.place.length < 3) return
    }
  },
  filters: {
    temperature: (value) => {
      return `${value.toFixed(0)}°`
    },
    date: (value) => {
      return (`0${value.getHours()}`).slice(-2) + ":" + (`0${value.getMinutes()}`).slice(-2)
    },
    time: (value) => {
      return (`0${value.getHours()}`).slice(-2) + ":" + (`0${value.getMinutes()}`).slice(-2)
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

.night {
  background: #16222a;
  background: linear-gradient(to bottom, #16222a, #3a6073);

  .search {
    background-color: #3a6073;

    &:active,
    &:focus {
      background-color: #335566;
    }
  }
}

.day {
  background: #56ccf2;
  background: linear-gradient(to bottom, #56ccf2, #2f80ed);

  .search {
    background-color: #56ccf2;

    &:active,
    &:focus {
      background-color: #41c5f1;
    }
  }
}

.orbit {
  position: absolute;
  background: url("./../assets/images/header.svg") no-repeat center center;
  background-size: 60vh; // border: 1px solid #000; // margin-top: 8%; // border-radius: 50%;
  height: 60vh;
  width: 60vh;
  align-self: center;
  transform: rotate(0deg); // animation: spin-right 10s linear infinite;
}

.sun {
  background-color: #ffc107;
  border-radius: 50px;
  margin-right: -40px;
  margin-bottom: -40px;
  height: 80px;
  position: absolute;
  right: 50%;
  bottom: 0;
  width: 80px;
}

.moon {
  background: url("./../assets/images/moon.svg") no-repeat;
  background-size: 60px;
  margin-left: -40px;
  margin-top: 40px;
  height: 60px;
  position: absolute;
  top: 0;
  left: 50%;
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
    width: 60vh;
    margin-top: 50px;
    display: flex;
    justify-content: center;
    flex-direction: column;
    .row {
      flex: 1;
    }

    .clock {
      display: inline-block;
      width: 50px;
      height: 50px;
    }
    .time {
      display: inline-block;
      height: 50px;
      line-height: 50px;
      font-size: 1.5rem;
      vertical-align: top;
      padding: 0 10px;
    }
  }

  .search {
    display: block;
    background: url("./../assets/images/loupe.svg") no-repeat center;
    background-size: 40px;
    width: 50px;
    height: 50px;
    border-radius: 50%;
    padding: 10px;
    box-shadow: 2px 2px 3.5px 0 rgba(0, 0, 0, .3);
    position: absolute;
    top: 0;
    left: 50%;
    transform: translate(-50%, -50%);
    transition: all .1s ease-in-out;

    &:hover {
      top: -1%;
    }
  }

  .search-box {
    display: flex;
    flex-direction: column;
    position: absolute;
    top: -120px;
    left: 50%;
    transform: translate(-50%, -50%);
    background: #fff;
    width: 300px;
    height: 100px;
    padding: 10px;
    justify-content: center;
    align-items: center;
    box-shadow: 2px 2px 3.5px 0 rgba(0, 0, 0, .3);

    &:after {
      content: ' ';
      display: block;
      position: absolute;
      width: 20px;
      height: 20px;
      bottom: -10px;
      left: 47%;
      background: #fff;
      transform: translate(-50%, -50%);
      transform: rotate(45deg);
      box-shadow: 2px 2px 3.5px 0 rgba(0, 0, 0, .3);
    }

    input {
      font-family: 'Dosis', sans-serif;
      font-size: 26px;
      height: 36px;
      padding: 0 10px; // flex: 1;
      &:focus,
      &:active {
        outline: none;
      }
    }

    label {
      font-size: 26px;
      margin-bottom: 10px;
    }
  }
}
</style>