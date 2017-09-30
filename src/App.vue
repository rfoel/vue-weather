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
  font-family: "Source Sans Pro", sans-serif;
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
  ul {
    list-style: none;
    li {
      img {
        height: 36px;
      }
    }
  }
}
</style>