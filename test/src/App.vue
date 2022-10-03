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
      if (this.city.trim().length > 2){
      return "Температура: " + this.info.main.temp
      }
    },
    showFeelsLike() {
      if (this.city.trim().length > 2){
      return "Ощущается как: " + this.info.main.feels_like
      }
    },
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = "Нужно название более одного символа"
        return false
      }
      this.error = ""
      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=853a0ef8b764c36d31b9691478828ec9`)
        .then(res => (this.info = res.data))
        
    }
  }
}
</script>

<template>
  <div class="wrapper">
    <h1>Погодное приложение</h1>
    <p>Узнать погоду в {{city == "" ?"вашем городе" : cityName}} </p>
    <input type="text" v-model="city" placeholder="Введите город">
    <button v-if="city != ''" @click="getWeather()">Получить погоду</button>
    <button disabled v-else>Введите погоду</button>
    <p class="error">{{error}}</p>

    <div v-if="info != null">
      <p>{{showTemp}} </p>
      <p>{{showFeelsLike}}</p>
    </div>
  </div>
</template>

<style scoped>
.error {
  color: crimson;
}

.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  padding: 20px;
  background: #1f0f24;
  text-align: center;
  color: #fff;
}

.wrapper h1 {
  margin-top: 50px;
}

.wrapper p {
  margin-top: 25px;
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

.wrapper button:disabled {
  background: #43b0ce;
  cursor: not-allowed
}

.wrapper button {
  background: #abcae3;
  color: #fff;
  border-radius: 10px;
  border: 2px solid #5abade;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper button:hover {
  transform: scale(1.1) translateY(-5px);
}
</style>
