<template>
  <div>
    <nav>
      <ul>
        <li>
          <img src="./assets/images/vue-weather.svg"> Vue Weather
        </li>
      </ul>
    </nav>
    <weather></weather>
  </div>
</template>

<script>

import Weather from "./components/Weather"

export default {
  name: "app",
  components: { Weather },
  mounted() { }
}
</script>

<style lang="scss">
html,
body {
  height: 100%;
  font-family: 'Dosis', sans-serif;
  color: #16222a;
}

body {
  display: flex;
  justify-content: center;
  overflow: hidden;
}

div {
  width: 100%;
}

nav {
  position: fixed;
  width: 100%;
  height: 56px;
  display: flex;
  background: linear-gradient(to bottom, rgba(0, 0, 0, 0.2) 0%, rgba(0, 0, 0, 0) 100%);
  align-content: center;
  justify-content: center;
  color: #fff;
  font-size: 1.5rem;
  ul {
    list-style: none;
    li {
      img {
        vertical-align: bottom;
        height: 30px;
      }
    }
  }
}
</style>