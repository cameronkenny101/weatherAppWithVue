<template>
  <div id="app">
    <main>
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="Enter Location ..." v-model="query" @keypress="fetchWeather"/>
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name}}, {{ weather.sys.country }}</div>
          <div class="date">{{ timestamp }}</div>
        </div>
      </div>

      <div class="weather-box"  v-if="typeof weather.main != 'undefined'">
        <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
        <div class="weather">{{ weather.weather[0].main }}</div>
      </div>

    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
    api_key: '4f2aac5504b52795efc1f4e1f69d524e',
    url_base: 'http://api.openweathermap.org/data/2.5/',
    query: '',
    weather: {},
    timestamp: ""
    }
  },
  created() {
    setInterval(this.getNow, 1000);
  },
  methods: {
    fetchWeather(e) {
      if(e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults)
      }
    },
    setResults (results) {
      this.weather = results; // hey
    },
    
    getNow: function() {
      const today = new Date();
      const date = today.getFullYear()+'-'+(today.getMonth()+1)+'-'+today.getDate();
      const time = today.getHours() + ":" + today.getMinutes() + ":" + today.getSeconds();
      const dateTime = date +' '+ time;
      this.timestamp = dateTime;
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: 'Raleway', sans-serif;
}
#app {
  background-image: url('./assets/alps.jpg');
  background-size: cover;
  background-position: bottom;
}
main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(rgba(120, 2232, 0, 0.75));
}
.search-box {
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar {
  font-family: 'Raleway', sans-serif;
  display: block;
  width: 100%;
  padding: 15px;
  text-align: center;
  color: black;
  font-size: 26px;
  appearance: none;
  border: none;
  outline: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 16px 16px 16px;
  margin-bottom: 50px;
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 32px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 16px 16px 16px;
}
.location-box .location {
  color: white;
  font-size: 48px;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date {
  color: whitesmoke;
  font-size: 18px;
  font-style: oblique;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.weather-box {
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: white;
  font-size: 192px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.15);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  padding: 12px 8px;
  color: white;
  font-size: 48px;
  font-style: oblique;
  font-weight: 600;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.05);
  border-radius: 16px;
  margin: 30px 0px;
}
</style>
