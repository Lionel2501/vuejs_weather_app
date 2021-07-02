<template>
  <div id="app">
    <div :class="isDay ? 'containerD' : 'containerN'">
    <main>
      <div class="search-box">
        <form action="" @submit.prevent="getWeather">
          <input type="text" class="search-bar" placeholder="Buscar.." v-model="citySearch">
        </form>
      </div>
      <div v-if="notFound" style="color:white; text-align:center;"
        >No se encontro la ciudad</div>
      <div class="weather-wrap">
        <div class="location-box">
        <div class="date">{{ dateBuilder() }}</div>
          <div style="display:flex;justify-content:center">
            <div class="location">{{ weather.city }}</div>
            <div class="country">{{ weather.country }}</div>
          </div>
        </div>
      </div>
      <div class="weather-box">
        <div class="temp">{{ weather.temp }}°c</div>
        <div class="description">{{ weather.description }}</div>
      </div>
    </main>
    </div>
  </div>
</template>

<script>
export default {
  data(){
    return{
      citySearch:'',
      weather:{
        city:'',
        country:'',
        date:'',
        temp:'',
        description:''
      },
      isDay: false,
      notFound: false
    }
  },
  methods: {
    getWeather: async function(){
      try {
        const key = '6bff45d6fbbb8b2a149ee276e80b05d5'
        const baseUrl = `http://api.openweathermap.org/data/2.5/weather?q=${this.citySearch}&appid=${key}&units=metric`
        
        const response = await fetch(baseUrl)
        const data = await response.json()
        console.log(data)

        this.weather.city = data.name
        this.weather.country = data.sys.country
        this.weather.temp = data.main.temp
        this.weather.description = data.weather[0].description      
        
        const dayOf = data.weather[0].icon

        if (dayOf.includes('n')) {
          this.isDay = false
        } else {
          this.isDay = true
        }
      } catch (error) {
        this.notFound = true
      }
    },
    dateBuilder(){
      let d = new Date()
      let months = ['Enero', 'Febrero', 'Marzo', 'Abril', 'Mayo', 'Junio', 'Julio', 'Agosto', 
        'Septiembre', 'Octubre', 'Noviembre', 'Diciembre']
      let days = ['Domingo', 'Lunes', 'Martes', 'Miercoles', 'Jueves', 'Viernes', 'Sabado']

      let day = days[d.getDay()]
      let date = d.getDate()
      let month = months[d.getMonth()]
      let year = d.getFullYear()

      return `${day}, ${date}, ${month}, ${year}`
    }
  },

}
</script>

<style>
*{
  margin:0;
  padding:0;
  box-sizing: border-box;
}

body{
  font-family: 'monserrat', sans-serif;
}
#app{
  justify-content: center;
  display: flex;
  border-radius: 5px;
  background: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.containerN{
  background-image: url('./assets/cold-bg.jpg');
}

.containerD{
  background-image: url('./assets/warm-bg.jpg');
}

main{
  background: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
  min-height:100vh;
  padding: 25px;
  background-position: bottom;
  background-size: auto;
  transition:0.4s;
  width: 360px;
}

.search-box{
  width:100%;
  margin-bottom: 30px;
}

.search-bar{
  width: 100%;
  padding: 10px;
  color: #313131;
  font-size: 20px;

  outline: none;
  background: none;
  appearance: none;
  border: none;
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 5px 20px 5px 20px;
  box-shadow: 0 0 8px;
}

.search-bar:focus{
  background-color: rgba(255, 255, 255, 1);
  transition: 0.4s;
  box-shadow: 0 0 16px;
  border-radius: 20px 5px 20px 5px;
}

.location-box, .location{
  color: #fff;
  font-size: 32px;
  font-weight: bold;
  text-align: center;
  text-shadow: 0 1px rgba(0, 0, 0, 0.5);
}

.country{
  color: #fff;
  font-size: 15px;
  font-weight: 300;
  margin: 15px 0 0 15px;
}
.date{
  color: #fff;
  opacity: 80%;
  font-size: 15px;
  font-weight: 300;
  text-align: center;
  text-shadow: rgba(255, 255, 255, 0.5);
  margin-bottom: 10px;
}

.temp{
  font-size: 50px;
  font-weight: 500;
  color: #fff;
  text-align: center;
  margin-top: 5rem;
  background: rgba(255, 255, 255, 0.20);
  box-shadow: 10px;
  padding: 30px;
  margin: 30px 50px;
  border-radius: 10px;
}

.description{
  font-size: 30px;
  font-weight: 300;
  color: #fff;
  text-align: center;
  margin-top: 5rem;
  padding: 10px;
  border-radius: 10px;
}
</style>
