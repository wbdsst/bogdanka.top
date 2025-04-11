<script>
import axios from 'axios'

export default {
  data() {
    return {
      city: "",
      error: "",
      info: null
    }
  },
  computed: {
    cityName() {
      return "«" + this.city + "»"
    },
    showTemp() {
      return "Температура: " + this.info.main.temp
    },
    showFeelsLike() {
      return "Ощущается как: " + this.info.main.feels_like
    },
    showMinTemp() {
      return "Минимальная температура: " + this.info.main.temp_min
    },
    showMaxTemp(){
      return "Максимальная температура: " + this.info.main.temp_max
    }
  },
  methods: {
    getWeather() {
      if(this.city.trim().length < 2) {
        this.error = "Нужно название более одного символа"
        return false
      }

      this.error = ""

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=e8764c67fb51999179bec2b865281007`)
        .then(res => (this.info = res.data))
    }
  }
}
</script>

<template>
  <main>
    <div class="wrapper">
      <h1>Погодное приложение</h1>
      <p>Узнать погоду в {{ city == "" ? "вашем городе" : cityName }}</p>
      <input type="text" v-model="city" placeholder="Введите город">
      <button v-show="city != ''" @click="getWeather()">Получить погоду</button>
      <p class="error">{{ error }}</p>
      <div v-if ="info != null && city != ''">
        <p>{{ showTemp }}</p>
        <p>{{ showFeelsLike }}</p>
        <p>{{ showMinTemp }}</p>
        <p>{{ showMaxTemp }}</p>
      </div>
    </div>
  </main>
</template>

<style scoped>
.error {
  color: #d03939;
}

.wrapper {
  width: 100svh;
  height: 60svh;
  border-radius: 50px;
  background-color: #1f0f24;
  padding: 20px;
  text-align: center;
  color: white;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
}

.wrapper h1 {
  margin-top: 50px;
}

.wrapper p {
  margin-top: 20px;
}

.wrapper input {
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid #110813;
  color: #fcfcfc;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;

}

.wrapper input:focus {
  border-bottom-color: #6e2d7d;
}

.wrapper button {
  background: #4d13a5;
  color: #fff;
  border-radius: 10px;
  border: 4px solid #540095;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper button:hover {
  transform: scale(1.1);
}
</style>