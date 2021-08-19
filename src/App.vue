<template>
  <div class="body">
    <div v-if="weatherData.main" class="container">
      <div class="c1">
        <h1 class="heading">Vue Weather</h1>
        <div class="temp-container">
          <h1 class="temperature">{{ Math.round(weatherData.main.temp) }}°c</h1>
          <div>
            <div class="city">{{weatherData.name}}</div>
            <div class="date">{{date}}</div>
          </div>
          <div class="icon">
            <!-- https://openweathermap.org/img/wn/${weatherData.weather[0].icon}@4x.png -->
            <img
              :src="`https://openweathermap.org/img/wn/${weatherData.weather[0].icon}@4x.png`"
              alt="weather image"
            />
            <p class="weather">{{ weatherData.weather[0].main }}</p>
          </div>
        </div>
      </div>
      <div class="temp-details">
        <div class="search-box">
          <span class="material-icons">search</span>
          <input type="text" placeholder="Search City" />
        </div>
        <h4>Weather Details</h4>
        <div class="details">
          <div class="item">
            <p class="key">Cloudy</p>
            <p class="value">{{weatherData.clouds.all}}%</p>
          </div>
          <div class="item">
            <p class="key">Humidity</p>
            <p class="value">{{weatherData.main.humidity}}%</p>
          </div>
          <div class="item">
            <p class="key">Wind</p>
            <p class="value">{{Math.round(weatherData.wind.speed)}}m/s</p>
          </div>
          <div class="item">
            <p class="key">Pressure</p>
            <p class="value">{{weatherData.main.pressure}}hPa</p>
          </div>
        </div>
      </div>
    </div>
    <div v-else class="loader">
    <h1>{{ respMsg }}</h1>
    <button v-if="respMsg !== 'Loading...'" @click="reload">Reload</button>
  </div>
  </div>
</template>

<script lang="ts" setup>
import { onMounted, ref } from "@vue/runtime-core";
import dayjs from "dayjs";

const searchCity = ref("");
const weatherData: any = ref("");
const date = dayjs().format("dddd, MMMM D, YYYY");
const respMsg = ref("Loading...");

function fetchData() {
  const q = searchCity.value ? searchCity.value : "Lahore";
  fetch(
    `https://api.openweathermap.org/data/2.5/weather?q=${q}&units=metric&appid=appID`
  )
    .then((response) => response.json())
    .then((data) => {
      searchCity.value = "";
      if (data.cod == 404) {
        respMsg.value = data.message;
        weatherData.value = "";
        return;
      }
      console.log(data);
      
      weatherData.value = data;
    });
}

function reload() {
  window.location.reload();
}

onMounted(() => {
  fetchData();
});
</script>
  

<style>
* {
  padding: 0;
  margin: 0;
  font-family: "Roboto", sans-serif;
  box-sizing: border-box;
}
.body {
  width: 100%;
  height: 100vh;
  background: linear-gradient(
    180deg,
    rgba(157, 197, 220, 1) 0%,
    rgba(47, 145, 203, 1) 100%
  );
  display: flex;
  justify-content: center;
  align-items: center;
}

.container {
  background: linear-gradient(
      263deg,
      rgba(255, 255, 255, 0.5) 0%,
      rgba(255, 255, 255, 0.5) 100%
    ),
    url("https://images.unsplash.com/photo-1517685352821-92cf88aee5a5?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1267&q=80")
      center/cover no-repeat fixed;
  width: 80%;
  border-radius: 5px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.3);
  height: 80%;
  display: flex;
  flex-direction: column;
  justify-content: center;
}
.c1 {
  width: 100%;
  height: 50%;
}
.temp-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-evenly;
  height: 100%;
}

.heading {
  display: none;
}

.temperature {
  font-size: 3rem;
}

.city {
  font-size: 3rem;
  letter-spacing: 3px;
  text-align: center;
  font-weight: 500;
}
.date {
  font-size: 14px;
  color: #666;
  text-align: center;
}
.icon {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
img {
  width: 50px;
}
.weather {
  font-size: 1.3rem;
}

.temp-details {
  background: rgba(0, 0, 0, 0.8);
  border-radius: 0 0 5px 5px;
  height: 50%;
  width: 100%;
  padding: 1rem;
  color: #eee;
}

.search-box {
  display: flex;
  justify-content: start;
  align-items: center;
  background: rgba(255, 255, 255, 0.1);
  padding: 0 1rem;
  border-radius: 50px 30px;
  color: #ddd;
}

.search-box span {
  margin-top: 1px;
}
.search-box input {
  outline: none;
  border: none;
  background: transparent;
  width: 100%;
  padding: 0.7rem 0.3rem;
  font-size: 1rem;
  color: #fff;
}

.search-box:focus-within {
  background: rgba(255, 255, 255, 0.2);
  box-shadow: 3px 3px 10px rgba(255, 255, 255, 0.2);
}

.temp-details h4 {
  margin-top: 2rem;
  font-weight: 500;
  font-size: 1.1rem;
}

.item {
  display: flex;
  justify-content: space-between;
  margin: 1rem 0;
  font-size: 0.8rem;
}
.item .key {
  color: rgba(255, 255, 255, 0.5);
}


.loader {
  display: flex;
  flex-direction: column;
  justify-content: center;
}
.loader h1 {
  color: #fff;
  font-size: 3rem;
  padding: 2rem;
}
.loader button:active {
  background: #ddd;
}
.loader button {
  font-size: 2rem;
  padding: 0.5rem 0;
  border-radius: 5px;
  background: #eee;
  border: none;
  cursor: pointer;
}

@media screen and (min-width: 800px) {
  .container {
    flex-direction: row;
    justify-content: space-between;
  }
  .c1 {
    height: 100%;
    padding-top: 3rem;
  }
  .temp-container {
    height: 100%;
    width: 60%;
  }
  .temp-details {
    height: 100%;
    width: 40%;
    border-radius: 0 5px 5px 0;
  }
  .heading {
    display: block;
    text-align: center;
    font-size: 3rem;
    color: #333;
  }
  .temp-container {
    flex-direction: row;
    justify-content: start;
    margin-left: 3rem;
    margin-top: 8rem;
  }
  .temp-container > * {
    margin: 0 1rem;
  }
}
</style>
