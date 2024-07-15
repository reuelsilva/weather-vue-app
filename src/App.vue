<template>
  <div class="box-container">
    <div class="box-header">
      <input type="text" name="query" v-model="query" @keyup.enter="fetchingData" placeholder="Digite o nome da cidade">
      <button @click="fetchingData"><i class="fa-solid fa-magnifying-glass"></i></button>
    </div>
    <div class="box-content" v-show="showInfos">
      <div class="box-temp">
        <img :src="'https://openweathermap.org/img/wn/' + tempIcon + '@2x.png'" alt="Icone do clima">
        <div class="box-temp-description">
          <p class="temp">{{ temp }}°C</p>
          <p class="description">{{ description }}</p>
        </div>
      </div>
      <p class="location"><i class="fa-solid fa-location-dot"></i> {{ location }}</p>
      <div class="extra-infos">
        <div class="info">
          <i class="fa-regular fa-sun"></i>
          <div>
            <p>{{ tempMax }}°C</p>
            <h2>Máx.</h2>
          </div>
        </div>
        <div class="info">
          <i class="fa-regular fa-snowflake"></i>
          <div>
            <p>{{ tempMin }}°C</p>
            <h2>Min.</h2>
          </div>
        </div>
        <div class="info">
          <i class="fa-solid fa-droplet"></i>
          <div>
            <p>{{ humidity }}%</p>
            <h2>Umidade</h2>
          </div>
        </div>
        <div class="info">
          <i class="fa-solid fa-wind"></i>
          <div>
            <p>{{ wind }}km/h</p>
            <h2>Vel. do vento</h2>
          </div>
        </div>
      </div>
    </div>

    <div class="not-found" v-show="showNotFound">
        <p>Cidade não encontrada.</p>
      </div>
  </div>
</template>

<script>
export default {
  name: 'App',  
  data(){
    return{
      query: "",
      showInfos: false,
      showNotFound: false,
      temp: "",
      description: "",
      tempIcon: "",
      location: "",
      tempMax: "",
      tempMin: "",
      humidity: "",
      wind: ""
    }
  },
  methods: {
    async fetchingData(){
      const API_KEY = "02f15d7d943c547b401971e975e7c6e4";

      try{ 
        const response = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${this.query}&units=metric&lang=pt_br&appid=${API_KEY}`);
        const data = await response.json();

        if(data.cod == 200){
          this.showInfos = true;
          this.showNotFound = false;

          this.temp = data.main.temp.toFixed(0);
          this.description = data.weather[0].description;
          this.location = data.name;
          this.tempIcon = data.weather[0].icon;
          this.tempMax = data.main.temp_max.toFixed(0);
          this.tempMin = data.main.temp_min.toFixed(0);
          this.humidity =  data.main.humidity.toFixed(0);
          this.wind = data.wind.speed.toFixed(0);
        }
        else{
          this.showInfos = false;
          this.showNotFound = true;
        }
      } catch(error){
        this.showInfos = false;
        this.showNotFound = true;
      }
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@500&display=swap');

*{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  font-family: "Poppins", sans-serif;
}

:root{
    --color-01: #001331;
    --color-02: #626E6E;
    --color-03: #EBFFFD;
    --color-04: #bd0000;
    --stop-01: rgba(0,255,185,1);
    --stop-02: rgba(92,83,138,1);
}

#app{
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 1rem;
  background-color: var(--color-01);
}

.box-container{
  max-width: 600px;
  width: 100%;
  padding: 1rem;
  background: linear-gradient(150deg, var(--stop-01) 10%, var(--stop-02) 100%);
  border-radius: 0.8rem;
}

.box-container .box-header{
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 1rem;
  margin-bottom: 2rem;
}

.box-container .box-header input{
  width: 100%;
  max-width: 300px;
  padding: 1rem;
  border-radius: 50px;
  background-color: var(--color-03);
}

.box-container .box-header input:focus{
  outline: none;
}

.box-container .box-header button{
  cursor: pointer;
  width: 52px;
  height: 52px;
  flex-shrink: 0;
  border-radius: 50%;
  background-color: var(--color-03);
}

.box-container .box-header input, .box-container .box-header button{
  font-size: 1rem;
  color: var(--color-02);
  border: none;
}

.box-container .box-content .box-temp{
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 1rem;
}

.box-container .box-content .box-temp img{
  filter: drop-shadow(2px 2px 2px rgba(0, 0, 0, 0.2));
}

.box-container .box-content .box-temp .temp{
  font-size: 2.5rem;
  color: #ffffff;
  line-height: 3rem;
}

.box-container .box-content .box-temp .description{
  color: #ffffff;
  max-width: 200px;
  text-transform: capitalize;
}

.box-container .box-content .location{
  font-size: 1.8rem;
  color: #ffffff;
  text-align: center;
  margin-bottom: 1rem;
}

.box-container .box-content .extra-infos{
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-gap: 1rem;
  max-width: 300px;
  width: 100%;
  margin: 0 auto;
}

.box-container .box-content .extra-infos .info{
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 1rem;
  padding: 0.5rem;
  color: #ffffff;
  background: #00000054;
  border-radius: 0.5rem;
}

.box-container .box-content .extra-infos .info i{
  font-size: 1.2rem;
  text-align: center;
}

.box-container .box-content .extra-infos .info h2{
  font-size: 0.8rem;
  font-weight: 500;
  text-align: center;
}

.box-container .not-found{
  color: var(--color-04);
  text-align: center;
}
</style>