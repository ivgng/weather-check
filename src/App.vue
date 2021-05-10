<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{dateBuilder()}}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data () {
    return {
      api_key: '13ce9ee4fe77ad701b0c4b17cd90887f',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e){
      if(e.key == "Enter"){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results){
      this.weather = results;
    },
    dateBuilder(){
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }
  }
}
</script>

<style>
*{
  margin:0;
  padding: 0;
  box-sizing: border-box;
}

body{
  font-family: 'poppins', sans-serif;
}

#app {
  background-image: url('./assets/img.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.6s;
}

#app.warm {
  background-image: url('./assets/img-2.jpg');
}

main{
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25) rgba(0,0,0,0.75));
}

.search-box{
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar{
  display:block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background:none;

  background-color: rgba(255,255,255, 0.5);
  border-radius: 10px;
  transition: 0.4s;
}

.search-box .search-bar:focus{
  box-shadow: 0px 0px 16px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255, 0.5);
  border-radius: 20px;
}

.location-box .location{
  color: #fff;
  font-size: 30px;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0,0.75);
}

.location-box .date{
  color: #fff;
  font-size: 20px;
  text-align: center;
}

.weather-box{
  text-align:center;
}

.weather-box .temp{
  display: inline-block;
  padding: 10px;
  color: #fff;
  font-size: 60px;
  font-weight: 900;

  text-shadow: 1px 3px rgba(0,0,0,0.75);
  background-color: rgba(255,255,255, 0.5);
  margin: 20px;
  border-radius: 20px;

  box-shadow: 1px 3px rgba(0,0,0,0.25);
}

.weather-box .weather{
  color: #fff;
  font-size: 30px;
  font-weight: 700;
  text-shadow: 1px 3px rgba(0,0,0,0.75);
}
</style>
