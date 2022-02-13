<template>
  <div class="about">
    <br><br>
    <div>
      <input placeholder="Linna nimi" v-model="city">
    </div>
    <div>
      <button v-on:click="findCurrentWeather">Leia hetke ilm</button>
    </div>
    <br>
    <div v-if="divVisibility">
      <table id="weatherTable">
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
    </div>
  </div>

</template>

<!-- 21:40 - 23:18 sh javaga pusimine, projektide alustamine jne-->

<script>
export default {
  name: "weather",
  data: function () {
    return {
      city: "",
      temp: "",
      wind: "",
      humidity: "",
      clouds: "",
      icon: "",
      divVisibility: false
    }
  },
  methods: {
    findCurrentWeather: function () {
      let city = this.city
      this.$http.get("https://api.openweathermap.org/data/2.5/weather?q=" + city + "&appid=d118f67c666260825a7a119163d5cac2&units=metric"
      )
          .then(response => {
            console.log(response.data)
            this.temp = response.data.main.temp
            this.wind = response.data.wind.speed
            this.humidity = response.data.main.humidity
            this.clouds = response.data.clouds.all
            this.icon = "https://openweathermap.org/img/w/" + response.data.weather[0].icon + ".png"

            this.divVisibility = true
            // this.weather = response.data
          })
          .catch(error => {
            console.log(error.response)
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

#weatherTable {
  text-align: left;
}
</style>