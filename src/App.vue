<template>
  <div id="app" :class="typeof weather.main != 'undefined' ? weather.weather[0].main : 'default' ">
    <div class="main_wrapper">
      <div class="search-container">
        <input 
        type="text" 
        class="search-input"  
        placeholder="Search..." 
        v-model="query"
        @keyup.enter="fetchWeather"
        />
      </div>
      <div class="weather-info" v-if="typeof weather.main != 'undefined' ">
        <div class="weather-location"><h3>{{weather.name}}, {{weather.sys.country}}</h3></div>
        <div class="weather-date">{{weather.weather[0].main}} wednesday 11 March 2020</div>
        <div class="weather-temp">{{ Math.round(weather.main.temp) }} °C</div>
        <div class="weather-description">
          <img :src="iconCode" />
          {{ weather.weather[0].description }}
        </div>
      </div>  
    </div>
  </div>
</template>
<!-- <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/axios/0.19.2/axios.min.js"></script> -->
<script>

export default {
  name: 'App',
  data(){
    return{
      api_key: '032bae216ae23de37c59713633fbd3d8', 
      url_basis: 'https://api.openweathermap.org/data/2.5/',
      query : '',
      weather:{}, 
      iconCode : ''
    }
  }, 
  methods:{
    fetchWeather(){
      // console.log(this.query);
      fetch(
        `${this.url_basis}weather?q=${this.query}&units=metric&appid=${this.api_key}`
      ).then(response => {
        return response.json();
      }).then(this.fetchData);
      this.query = '' ;
    },
    fetchData(results){
      this.weather = results ;
      this.iconCode = "http://openweathermap.org/img/w/"+ results.weather[0].icon+".png";
    }
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  font-family: sans-serif;
}
#app{
  background-position: bottom;
  background-size: cover;
}
.Clear{
  background: url('./assets/clear-bg.jpg');
}
.default{
  background: url('./assets/default.jpg');
}
.Clouds{
  background: url('./assets/clouds-bg.jpg');
}
.Thunderstorm{
  background: url('./assets/thunderstorm-bg.jpg');
}
.Rain{
  background: url('./assets/rain-bg.jpg');
}
.Snow{
  background: url('./assets/snow-bg.jpg');
}
.main_wrapper{
  height: 100vh;
  padding: 30px;
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  background-image: linear-gradient(to bottom, rgb(0,0,0,0.25), rgb(0,0,0,0.75));
}
.search-container{
  margin-bottom: 30px;
  flex: 1;
}

.search-input{
  width: 100%;
  padding: 16px;
  background: #ffffff4d;
  color: white;
  outline: none;
  border: none;
  border-top-left-radius: 14px;
  border-bottom-right-radius: 14px;

}
.search-input:focus{
  background: #ffffff21 !important; 
}
.weather-info{
  padding: 20px;
  width: 100%;
  flex: 3;
}
.weather-location h3{
  color:white;
  text-align: center;
  margin-bottom: 10px;
}
.weather-date{
  color: rgb(255,255,255,0.6);
  text-align: center;
  font-size: 14px;
  margin-bottom: 20px;
}
.weather-description{
  color: white;
  text-align: center;
  margin-bottom: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.weather-temp {
  padding: 7px 13px;
  margin: 0 auto;
  text-align: center;
  color: white;
  font-size: 53px;
  font-weight: bold;
  /*background: rgb(255,255,255,0.6);*/
  /*border-top-left-radius: 20px;*/
  /*border-bottom-right-radius: 20px;*/
}
::placeholder {
    color: white;
    font-size: 15px;
    opacity: 1;
}

</style>
