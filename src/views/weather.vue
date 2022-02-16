<template>
  <div class="about">
    <br><br>
    <div>
      <input placeholder="Linna nimi" v-model="cityInput">
    </div>
    <div>
      <button v-on:click="findCurrentWeather">Leia hetke ilm</button>
    </div>
    <br>
    <div v-if="mainDivVisibility">
      <table>
        <tr>
          <td>Temp:</td>
          <td>{{temp}} C</td>
        </tr>
        <tr>
          <td>Tuul:</td>
          <td>{{wind}} m/s</td>
        </tr>
        <tr>
          <td>Õhuniiskus:</td>
          <td>{{humidity}} %</td>
        </tr>
        <tr>
          <td>Pilvisus:</td>
          <td>{{clouds}} %</td>
        </tr>
      </table>
      <img :src=icon>
      <br>
      {{city}}, {{country}}
    </div>
    <div v-if="errorDivVisibility">Sisestatud linna ei eksisteeri või tegid trükkimisel vea. Proovi uuesti.</div>
  </div>

</template>

<!--todo: hetkel on "Rome" otsingu puhul tulemuseks Rome, US. Mõtle lahendus samade nimedega linnade puhul-->

<script>
export default {
  name: "weather",
  data: function () {
    return {
      cityInput: "",
      city: "",
      country: "",
      temp: "",
      wind: "",
      humidity: "",
      clouds: "",
      icon: "",
      mainDivVisibility: false,
      errorDivVisibility: false

    }
  },
  methods: {
    findCurrentWeather: function () {
      let cityInput = this.cityInput
      this.$http.get("https://api.openweathermap.org/data/2.5/weather?q=" + cityInput + "&appid=d118f67c666260825a7a119163d5cac2&units=metric"
      )
          .then(response => {
            console.log(response.data)
            this.temp = response.data.main.temp
            this.wind = response.data.wind.speed
            this.humidity = response.data.main.humidity
            this.clouds = response.data.clouds.all
            this.country = response.data.sys.country
            this.city = cityInput
            this.icon = "https://openweathermap.org/img/w/" + response.data.weather[0].icon + ".png"

            this.mainDivVisibility = true
            this.errorDivVisibility = false
            // this.weather = response.data
          })
          .catch(error => {
            this.mainDivVisibility = false
            this.errorDivVisibility = true
          })
    }
  }
}
</script>

<style>
input {
  height: 34px;
  font-size: 18px;
  text-align: center;
  background-color: transparent;
  border-width: 1px;
  border-color: rgba(0, 0, 0, 0.24);
}

button {
  width: 220px;
  border-width: 1px;
  border-color: rgba(0, 0, 0, 0.24);
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 18px;
  background-color: transparent;
  color: #2c3e50;
  padding: 8px 16px;
}

button:hover {
  box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}

body {
  background-color: aliceblue;
}

table{
  margin-left: auto;
  margin-right: auto;
  text-align: left;
}

td {
  padding: 7px;
  border-bottom: solid;
  border-width: 1px;
  border-color: lightgray;
}

th {
  padding: 10px;
  border: solid;
  border-width: 1px;
  border-color: lightgray;
  background-color: lightgray;
}

</style>