<template>
<div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp - 273.15 > 16 ? 'warm' : ''">
  <main>
    <div class="search-box">
      <input type="text" class="search-bar" placeholder="Search...." v-model="query" @keypress="fetchWeather">

    </div>

    <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
      <div class="location-box">
        <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
        <div class="date"> {{dateBuilder()}} </div>
      </div>
      <div class="weather-box">
        <div class="temp">{{ Math.round(changeTemp) }}°c</div>
        <div class="weather">{{ weather.weather[0].main }}</div>
      </div>
    </div>
  </main>
</div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      api_key: '186f219fba4a76d632ede27e82ddaa36',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      let d = new Date();
      let months = ['tháng 1 ', 'tháng 2 ', 'tháng 3 ', 'tháng 4 ', 'tháng 5 ', 'tháng 6 ', 'tháng 7 ', 'tháng 8 ', 'tháng 9 ',
        'tháng 10', 'tháng 11', 'tháng 12'
      ];
      let days = ['Thứ 2 ', 'Thứ 3 ', 'Thứ 4 ', 'Thứ 5 ', 'Thứ 6 ', 'Thứ 7 ', 'Chủ nhật '];
      let nameDate = ',ngày ';
      let nameYear = 'năm ';

      let day = days[d.getDay()];
      let date = nameDate + d.getDate();
      let month = months[d.getMonth()];
      let year = nameYear + d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    },

  },
  computed: {
    changeTemp(){
      return this.weather.main.temp - 273.15;
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
  font-family: 'montserrat', sans-serif;
}

#app {
  max-width: 500px;
  margin: 0 auto;
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url('./assets/warm-bg.jpg');
}
main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  background-color: rgba(255, 255, 255, 0.75);
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  border-radius: 16px 0px 16px 0px;
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  padding-bottom: 5px;
}

.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.2);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25)
}


</style>
