<template>
  <div id="app">
      <div class="main">
        <div class="container my-5">
          <h1 class="title text-center">Погода в городе <span>{{ weather.cityName }}</span></h1>
          <div class="card rounded my-3 shadow-lg back__card overflow-hidden">
            <div>
              <div icon="sunny" v-if="clearSky">
                <span class="sun"></span>
              </div>
              <div icon="snowy" v-if="snowy">
                <ul>
                  <li></li>
                  <li></li>
                  <li></li>
                  <li></li>
                  <li></li>
                  <li></li>
                  <li></li>
                  <li></li>
                  <li></li>
                  <li></li>
                  <li></li>
                  <li></li>
                  <li></li>
                </ul>
              </div>
              <div icon="stormy" v-if="stormy">
                <span class="cloud"></span>
                <ul>
                  <li></li>
                  <li></li>
                  <li></li>
                  <li></li>
                  <li></li>
                </ul>
              </div>
              <div icon="cloudy" v-if="cloudy">
                <span class="cloud"></span>
                <span class="cloud"></span>
              </div>
              <div icon="nightmoon" v-if="clearNight">
                <span class="moon"></span>
                <span class="meteor"></span>
              </div>
            </div>
            <div class="card__top text-center" style="margin-bottom: 15rem">
              <div class="city__name my-3">
                <p>{{ weather.cityName }}</p>
                <span>...</span>
                <p class="">{{ weather.country }}</p>
              </div>
            </div>
            <div class="card__body">
              <div class="card__mid">
                <div class="row">
                  <div class="col-12 text-center temp">
                    <span>{{ weather.temperature }}&deg;C</span>
                    <p class="my-4">{{ weather.description }}</p>
                  </div>
                </div>
                <div class="row">
                  <div class="col d-flex justify-content-between px-5 mx-5">
                    <p>
                      <img src="./assets/down.svg" alt="" />
                      {{ weather.lowTemp }}&deg;C
                    </p>
                    <p>
                      <img src="./assets/up.svg" alt="" />
                      {{ weather.highTemp }}&deg;C
                    </p>
                  </div>
                </div>
              </div>
              <div class="card__bottom px-5 py-4 row">
                <div class="col text-center">
                  <p>{{ weather.feelsLike }}&deg;C</p>
                  <span>Ощущается как</span>
                </div>
                <div class="col text-center">
                  <p>{{ weather.humidity }}%</p>
                  <span>Влажность</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data: () => ({
    cityFound: false,
    visible: false,
    stormy: false,
    cloudy: false,
    clearSky: false,
    clearNight: false,
    snowy: false,
    isDay: true,
    lat: '',
    lon: '',
    weather: {
      cityName: '',
      country: '',
      temperature: '',
      description: '',
      lowTemp: '',
      highTemp: '',
      feelsLike: '',
      humidity: '',
    }
  }),
  async mounted () {
    const coordinates = await this.$getLocation()
    this.lat = coordinates.lat
    this.lon = coordinates.lng
    const key = '9a6410528a54374282ebe88d16fcd19c'
    const baseURL = `http://api.openweathermap.org/data/2.5/weather?lat=${this.lat}&lon=${this.lon}&cnt=1&appid=${key}&units=metric&lang=Ru`
    const response = await fetch(baseURL)
    const data = await response.json()
    this.weather.cityName = data.name
    this.weather.country = data.sys.country
    this.weather.temperature = Math.round(data.main.temp)
    this.weather.description = data.weather[0].description
    this.weather.lowTemp = Math.round(data.main.temp_min)
    this.weather.highTemp = Math.round(data.main.temp_max)
    this.weather.feelsLike = Math.round(data.main.feels_like)
    this.weather.humidity = Math.round(data.main.humidity)

    const timeOfDay = data.weather[0].icon;
    if (timeOfDay.includes("n")) {
      this.isDay = false;
    } else {
      this.isDay = true;
      }

    const mainWeather = data.weather[0].main;
    if (mainWeather.includes("Clouds")) {
      this.stormy = false;
      this.cloudy = true;
      this.clearSky = false;
      this.clearNight = false;
      this.snowy = false;
    }
    if (mainWeather.includes("Clouds")) {
      this.stormy = false;
      this.cloudy = true;
      this.clearSky = false;
      this.clearNight = false;
      this.snowy = false;
    }
    if (
      mainWeather.includes("Thunderstorm") ||
      mainWeather.includes("Rain")
    ) {
      this.stormy = true;
      this.cloudy = false;
      this.clearSky = false;
      this.clearNight = false;
      this.snowy = false;
    }
    if (mainWeather.includes("Clear") && this.isDay) {
      this.stormy = false;
      this.cloudy = false;
      this.clearSky = true;
      this.clearNight = false;
      this.snowy = false;
    }
    if (mainWeather.includes("Clouds") && !this.isDay) {
      this.stormy = false;
      this.cloudy = false;
      this.clearSky = false;
      this.clearNight = true;
      this.snowy = false;
    }
    if (mainWeather.includes("Snow")) {
      this.stormy = false;
      this.cloudy = false;
      this.clearSky = false;
      this.clearNight = false;
      this.snowy = true;
    }
    this.visible = true;
    this.cityFound = false;
  }
}
</script>

<style>
@import "./assets/custom.css";
@import "./assets/animation.css";
</style>
