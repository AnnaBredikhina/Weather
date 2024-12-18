<script>
import axios from 'axios';

export default {
  data() {
    return {
      city: "",
      error: "",
      info: null
    }
  },
  methods: {
    getWeather() {
      if(this.city.trim().length < 2) {
        this.error = "Название должно быть более одного символа";
        return false;
      }

      this.error = "";
      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&lang=ru&APPID=916c2c3249015be677b306fd675c6b54`)
        .then(response => (this.info = response.data))
    },

    rounding(characteristic) {
      return Math.round(characteristic)
    }
  }
}
</script>

<template>
  <div className="wrapper">
    <h1>Погода</h1>
    <p v-if="!city">Узнать погоду в вашем городе</p>
    <p v-else>Узнать погоду в городе «{{ city }}»</p>
    <input type="text" placeholder="Введите город" v-model="city">
    <button type="button" v-if="!city" disabled>Получить погоду</button>
    <button type="button" v-else @click="getWeather()">Получить погоду</button>
    <p className="error">{{ error }}</p>

    <div v-if="info" className="weatherInfo">
      <p><span>Описание:</span> {{ info.weather[0].description }}</p>
      <p><span>Температура сейчас:</span> {{ rounding(info.main.temp) }} °C</p>
      <p><span>Ощущается как:</span> {{ rounding(info.main.feels_like) }} °C</p>
      <p><span>Ветер:</span> {{ rounding(info.wind.speed) }} м/с</p>
      <p><span>Влажность:</span> {{ rounding(info.main.humidity) }} %</p>
    </div>

  </div>
</template>

<style scoped>
.error {
  color: rgb(152, 0, 0);
}

.wrapper {
  box-sizing: border-box;
  width: 80vw;
  min-width: 260px;
  height: 500px;
  border-radius: 40px;
  background: rgba(242, 242, 242, 0.4);
  border: 1px solid #fff;
  padding: 30px;
  text-align: center;
  color: rgb(0, 72, 69);
  position: relative;
}

.wrapper p {
  margin-top: 20px;
}

.wrapper input {
  margin: 30px 0 15px;
  background: transparent;
  border: 0;
  border-bottom: 1px solid #fff;
  color: rgb(0, 72, 69);
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
  transition: all 0.2s;
}

.wrapper input:focus {
  border-bottom-color: rgb(0, 72, 69);
}

.wrapper button {
  background: rgba(0, 163, 155, 0.6);
  color: #fff;
  padding: 10px;
  cursor: pointer;
  border: 1px solid #fff;
  border-radius: 4px;
  margin-left: 20px;
  transition: all 0.3s;
}

.wrapper button:disabled {
  cursor: not-allowed;
}

.wrapper button:hover {
  background: rgba(0, 163, 155, 0.8);
  transform: scale(1.05);
}

.weatherInfo span {
  font-weight: 600;
}
</style>
