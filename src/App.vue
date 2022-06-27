<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <div class="about-app">
        <h1>Como funciona?</h1>
        <ul class="steps-list">
          <li> - Clique no campo de busca e escreva o nome de uma cidade do mundo</li>
          <li> - Pressione a tecla "Enter" no seu teclado e veja o clima e a temperatura do local escolhido aparecer!</li>
        </ul>
        <div class="agree">
          <button class="button" v-on:click="closeAboutApp()"><span>Entendi</span></button>
        </div>
      </div>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Busque por cidade, estado ou país"
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
      <span>teste</span>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      api_key: '8000a251af8997034a322db0b2692a6f',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
    },
    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
    closeAboutApp(){
      document.querySelector(".about-app").style.display = "none";
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

ol,ul{
  margin:0;
  padding:0;
}

li{
  list-style-type: none;
}

#app {
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
  padding: 15px 30px;
  color: #000000;
  font-size: 20px;
  appearance: none;
  border:none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.3);
  border-radius: 50px;
  transition: 0.4s;
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.6);
}

.search-box .search-bar::placeholder{
  color: rgba(255, 255, 255, 0.4);
}

.location-box .location {
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date {
  color: #FFF;
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
  color: #FFF;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.about-app{
  display: block;
  margin: 25px auto;
  padding: 20px;
  border-radius: 15px;
  background-color:rgba(255, 255, 255, 0.25);
  transition: 0.4s;
}

.about-app h1{
  margin-bottom: 15px;;
  color: #FFF;
  text-align: center;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.about-app ul{
  margin-bottom: 20px;
}

.about-app ul li{
  margin-bottom: 10px;
  color: #fff;
}

.about-app ul li:last-child{
  margin-bottom: 0;
}

.agree{
  display: flex;
  justify-content: center;
}

.agree button{
  width: auto;
  padding: 15px 70px;
  font-size: 24px;
  border: 0;
  border-radius: 50px;
  background-color: rgb(71, 255, 71);
  cursor: pointer;
  transition: 0.4s;
}

.agree button:hover{
  background-color: lightgreen;
}

@media only screen and (min-width:769px){
  .about-app{
    width: 50vw;
  }

}

@media only screen and (max-width:768px){
  .about-app h1{
    font-size: 25px;
  }

}

</style>