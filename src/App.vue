<script>
import axios from "axios";
export default {
  data() {
    return {
      city: "",
      error: "",
      info: null,
    };
  },
  computed: {
    cityName() {
      return this.city;
    },
    showTemp() {
      return `${this.info.main.temp}°C`;
    },
    showFeelsLike() {
      return `Ощущается как: ${this.info.main.feels_like}°C`;
    },
    showMinTemp() {
      return `Минимальная температура: ${this.info.main.temp_min}°C`;
    },
    showMaxTemp() {
      return `Максимальная температура: ${this.info.main.temp_max}°C`;
    },
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = "Нужно название больше одного символа!";
        return false;
      }

      this.error = "";
      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=2d3cc4c3d2a95dea5872920913d9fdd7`
        )
        .then((res) => (this.info = res.data));
    },
  },
};
</script>

<template>
  <div class="container">
    <div class="weather-box" v-if="info">
      <h1>{{ cityName }}</h1>
      <p class="temp">{{ showTemp }}</p>
      <p class="details">{{ showFeelsLike }}</p>
      <p class="details">{{ showMinTemp }}</p>
      <p class="details">{{ showMaxTemp }}</p>
    </div>
    <div class="input-box">
      <h1>Погодное приложение</h1>
      <p>Узнать погоду в городе {{ city || 'вашем городе' }}</p>
      <input type="text" v-model="city" placeholder="Введите город" />
      <button @click="getWeather()" :disabled="!city">Получить погоду</button>
      <p class="error">{{ error }}</p>
    </div>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 180px;
  padding: 10px;
  color: #333;
  font-family: Arial, sans-serif;
}

.weather-box, .input-box {
  width: 100%;
  max-width: 400px;
  background: rgba(255, 255, 255, 0.9);
  border-radius: 20px;
  padding: 20px;
  margin: 10px;
  text-align: center;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.weather-box h1, .input-box h1 {
  margin-bottom: 20px;
  font-size: 24px;
}

.temp {
  font-size: 48px;
  font-weight: bold;
  margin: 20px 0;
}

.details {
  margin: 10px 0;
  font-size: 18px;
}

.input-box p {
  margin-bottom: 20px;
  font-size: 16px;
}

input[type="text"] {
  padding: 10px;
  border: 2px solid #ccc;
  border-radius: 5px;
  width: 100%;
  margin-bottom: 20px;
}

button {
  background: #007bff;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background 0.3s ease;
  width: 100%;
}

button:disabled {
  background: #ccc;
}

button:not(:disabled):hover {
  background: #0056b3;
}

.error {
  color: #d03939;
  font-size: 14px;
}

@media (min-width: 768px) {
  .container {
    flex-direction: row;
    justify-content: space-around;
  }

  .weather-box, .input-box {
    width: 45%;
    max-width: none;
  }
}
</style>
