<template>
  <div id="app">
    <div class="main" v-bind:class="{ warm: state_weather }">
      <div class="search-box">
        <div class="search-bar">
          <div class="box">
            <input
              class="search-input"
              type="text"
              placeholder="Search..."
              v-model="data.city"
              @keyup.enter="getApi()"
            />
            <button
              class="button-favorite"
              title="Adicionar aos favoritos"
              v-bind:class="{ check: isFavorite }"
            >
              <fa icon="star" />
            </button>
          </div>
          <div v-if="data.weather">
            <!--header-->
            <div class="header">
              <h1>{{ data.weather.name }}</h1>
              <h3>{{ new Date().toLocaleString() }}</h3>
            </div>
            <!--temp-->
            <div class="temp">
              <h2>{{ Math.round(data.weather.main.temp) }}&deg;</h2>
            </div>
            <div class="state">
              <h3>{{ data.weather.weather[0].main }}</h3>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "WeatherA",
  props: {},
  data() {
    return {
      data: {
        weather: null,
        city: "",
        current_day: "",
        state_weather: false,
        api_key: "e6560ce1d36da08737f80f5eff4365ff",
      },
    };
  },
  mounted: async function () {
    this.getApi();
  },
  methods: {
    async getApi() {
      if (this.data.city === "") {
        this.data.city = "Fortaleza";
      }
      const getWeather = await axios.get(
        `https://api.openweathermap.org/data/2.5/weather?units=metric&q=${this.data.city}&appid=${this.data.api_key}`
      );
      this.data.weather = getWeather.data;
      this.data.city = "";
      if (this.data.weather.main.temp > 25) {
        this.state_weather = true;
      } else {
        this.state_weather = false;
      }
    },
  },
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "monsterrat", sans-serif;
}
#app {
  background-image: url("../assets/4.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
.main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(18, 75, 156, 0.35),
    rgba(2, 15, 22, 0.75)
  );
}
.main.warm {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(248, 8, 8, 0.35),
    rgba(246, 6, 6, 0.75)
  );
}
.search-box {
  width: 100%;
  margin: 20px;

  position: relative;
}
.search-box .search-bar {
  display: block;
  width: 20%;
  padding: 15px;
}
.box {
  display: flex;
  height: min-content;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px;
  background-color: rgba(255, 255, 255, 0.75);
}
.temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.header {
  padding-top: 20px;
  font-size: 20px;
  color: azure;
  box-shadow: rgba(3, 3, 3, 0.3);
}
.state {
  position: absolute;
  color: #fff;
  font-size: 48px;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  text-align: center;
}

.button-favorite {
  margin: 0;
  padding: 0;
  border: none;
  background: none;
  cursor: pointer;

  color: #fff;
}

.button-favorite ::hover {
  color: #2b2b2b;
}

.button-favorite .check {
  color: #f8f8f8;
}
</style>
