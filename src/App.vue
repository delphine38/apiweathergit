<template>
  <!-- ici je dis que si le type main est différent de différent et que le type main de température est plus élevé que 16 on lui attribut le style 'warm' -->
  <div
    id="app"
    :class="
      typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''
    "
  >
    <main>
      <div class="search-box">
        <div class="text-center">
          <h1 class="text-center">Indiquer une ville</h1>

          <input
            type="text"
            class="search-bar"
            placeholder="Search...."
            v-model="query"
            @keypress="fetchWeather"
            style="justify-content-center"
          />
        </div>

        <!-- {{ query }} -->
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            <h1>
              Voici la ville de : <span>{{ weather.name }}</span>
            </h1>
          </div>

          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">Il fait : {{ weather.main.temp }} °c</div>
          <div class="weather">
            Avec un temps : {{ weather.weather[0].main }}
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
/**import HelloWorld from "./components/HelloWorld.vue";***/

export default {
  name: "app",
  data() {
    return {
      apikey: "568f81b785a57763bf4a2a3eb14965f2",

      urlBase: `https://api.openweathermap.org/data/2.5/`,
      /* urlBase: "https://api.openweathermap.org/data/2.5/",**/
      query: "",
      weather: {},
    };
  },
  methods: {
    fetchWeather(e) {
      /** si tu appui sur "enter" */
      if (e.key == "Enter") {
        /** tu reois cette urlBase je te questionne sur cette query (où l'utilisateur marque une ville) de cette api */
        fetch(
          `${this.urlBase}weather?q=${this.query}&units=metric&APPID=${this.apikey}`
        )
          /** quand tu reçois le res , tu return res en json */
          .then((res) => {
            return res.json();
          })

          .then(this.setResults)

          /** */

          /** et affiche en log */
          .then((res) => {
            console.log("res : ", res);
            this.weather = res;
          })
          /** affiche les erreurs en log */
          .catch((e) => {
            console.log("erreur : ", e);
          });
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "Janvier",
        "Février",
        "Mars",
        "Avril",
        "Mai",
        "Juin",
        "Juillet",
        "Aout",
        "Septembre",
        "Octobre",
        "Novembre",
        "Décembre",
      ];
      let days = [
        "Lundi",
        "Mardi",
        "Mercredi",
        "Jeudi",
        "Vendredi",
        "Samedi",
        "Dimanche",
      ];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "montserrat";
}
#app {
  background-image: url("./assets/montagne.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
#app.warm {
  background-image: url("./assets/montagnechaaud.png");
}
main {
  min-height: 100vh;
  padding: 25px;
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: black;
  font-size: 40px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: rgba(255, 255, 255, 0.75);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0, 4s;
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}
.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500px;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300px;
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
  background-color: rgba(0, 0, 0, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700px;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
