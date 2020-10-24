<template>
  <div id="app">
      <div id="main">
        <div class="container my-5">
          <h1 class="title text-center">Погода в городе <span>Ульяновск</span></h1>
          <div class="card rounded my-3 shadow-lg back-card overflow-hidden">
            <div class="card-top text-center" style="margin-bottom: 15rem">
              <div class="city-name my-3">
                <p>{{ weather.cityName }}</p>
                <span>...</span>
                <p class="">{{ weather.country }}</p>
              </div>
            </div>
            <div class="card-body">
              <div class="card-mid">
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
              <div class="card-bottom px-5 py-4 row">
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
    weather: {
      cityName: "Ульяновс",
      country: "Ru",
      temperature: 12,
      description: "Облачно",
      lowTemp: "19",
      highTemp: "30",
      feelsLike: "20",
      humidity: "55",
    }
  })
}
</script>

<style>

</style>
