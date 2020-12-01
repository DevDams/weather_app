<template>
  <div id="app">
    <div id="navbar">
      <div class="app_title">
        Ma méteo 🌥🌼
      </div>
    </div>

    <main>
      <div class="serach_box">
        <input
          type="text"
          class="search_bar"
          placeholder="Chercher une ville..."
          v-model="query"
          @keypress="fetchWeather"
        >
      </div>

      <div class="weather_wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location_box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather_box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>

      <div class="description">
        <h3>Comment ça marche ?</h3><br>
        <p>
          Pour trouver la température en une ville donnée : <br>
          Entrez le nom de la ville dans la barre de recherche,
          et patientez quelques secondes...
        </p>
      </div>
    </main>

    
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      api_key: '4904538d41bfc39b8c10759f5776286c',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key == 'Enter') {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
         .then(res => {
           return res.json()
         })
         .then(this.setResults)
      }
    },
    setResults (results) {
      this.weather = results
    },
    dateBuilder () {
      let d = new Date()
      let months = ["Janvier", "Février", "Mars", "Avril",
      "Mai", "Juin", "Juillet", "Aout", "Septembre", "Octobre",
      "Novembre", "Décembre"]
      let days = ["Dimanche", "Lundi", "Mardi", "Mercredi",
      "Jeudi", "Vendredi", "Samedi"]

      let day = days[d.getDay()]
      let date = d.getDate()
      let month = months[d.getMonth()]
      let year = d.getFullYear()

      return `${day} ${date} ${month} ${year}`
    }
  },
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,300;0,400;0,600;0,700;0,800;0,900;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Montserrat', sans-serif;
}


#navbar {
  position: absolute;
  width: 100%;
  height: 60px;
  background: #000;
  color: #f2f2f2;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1;
}

.app_title {
  font-size: 25px;
  font-weight: 700;
  font-family: 'Montserrat', sans-serif;
}


#app {
  background-image: url('./assets/nightt.png');
  background-size: cover;
  background-position: bottom;
  background-repeat: no-repeat;
  transition: .4s;
}

main {
  position: relative;
  min-height: 700px;
  padding: 60px 25px 0 25px;
  background-image: linear-gradient(to bottom, rgba(65, 65, 65, 0.25), rgba(0, 0, 0, 0.479));
  
}

.search_box {
  width: 100%;
  margin-bottom: 30px;
}

.search_bar {
  display: block;
  font-family: 'Montserrat', sans-serif;
  font-weight: 400;
  width: 100%;
  margin-top: 50px;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  border-radius: 7px;
  border: none;
  appearance: none;
  box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
  background: none;
  background-color: rgba(255, 255, 255, 0.5);
  transition: .4s;
}

.search_bar:focus {
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.377);
  background-color: rgba(255, 255, 255, 0.75);
}


.weather_wrap {
  margin-top: 50px;
}

.location {
  color: #f2f2f2;
  font-size: 32px;
  font-weight: 600;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, .25);
}

.date {
  color: #f2f2f2;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather_box {
  text-align: center;
}

.temp {
  display: inline-block;
  padding: 10px 25px;
  color: #f2f2f2;
  font-size: 98px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, .25);
  background: rgba(255, 255, 255, .25);
  border-radius: 10px;
  margin: 30px 0;
  box-shadow: 3px 6px rgba(0, 0, 0, .25);
}

.weather {
  color: #f2f2f2;
  font-size: 42px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, .25);
}

.description {
  position: absolute;
  bottom: 10px;
  color: #f2f2f2;
}

.description p {
  font-size: 14px;
}


@media (min-height: 700px) {
  main {
    min-height: 100vh;
  }
}
</style>
