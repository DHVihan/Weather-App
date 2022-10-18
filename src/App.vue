<template>
    <div id="app" :class="this.imgPath">
    <main>
      <div class="search-box">
          <input 
            type="text" 
            class="search-bar" 
            placeholder="search..."
            v-model="query"
             @keypress="fetchWeather"
          />
       
        </div>
        <div class="weather-wrap" v-if= "typeof weather.main != 'undefined'">
          <div class="location-box">
            <div class="location">{{ weather.name}},{{ weather.sys.country}}</div>
            <div class="date">{{ dateBuider()}}</div>
          </div>

          <div class="weather-box"> 
            <div class="temp">{{ Math.round(weather.main.temp)}}°c</div>
            <br>
            <div class="pressure">Pressure </div>
            <div class="pressure">Humidity </div><br>
            
            <div class="pres"> {{ Math.round(weather.main.pressure)}} Pa</div>
            <div class="hum"> {{ Math.round(weather.main.humidity)}} %</div>
            <div class="weather">{{ weather.weather[0].description }} </div>
          </div>
        </div>
      
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
      data() {
    return {
      api_key : 'c1fb6084256df4bc68fb93d6e07ab495',
      url_base:'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather:{},
      imgPath: '',
      
    }
  },
   methods:{

    fetchWeather(e){
      if(e.key =="Enter"){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res =>{
            return res.json();
            
          }).then(this.setResults);

      }

    },
    setResults(results) {
      this.weather =results;
      this.getValue();
      // this.forceUpdate();
    },
    dateBuider(){
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July",
      "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    },

    getValue(){

      //console.log(this.weather);
     
      console.log(this.weather.weather[0].description);

      if(typeof this.weather.weather != 'undefined' && this.weather.weather[0].description == 'overcast clouds'){
        console.log('cloud');
        this.imgPath='cloud';
      }
      else if(this.weather.weather[0].description == 'clear sky'){
        //console.log('sun');
        this.imgPath='sun';
      }else if(this.weather.weather[0].description == 'light rain'){
        //console.log('rain');
        this.imgPath='rain';
      }else if(this.weather.weather[0].description == 'broken clouds'){
        //console.log('broke');
        this.imgPath='broke';
      }else if(this.weather.weather[0].description == 'scattered clouds'){
        //console.log('scat');
        this.imgPath='scat';
      }else if(this.weather.weather[0].description == 'heavy intensity rain'){
       // console.log('heavy');
        this.imgPath='heavy';
      }else if(this.weather.weather[0].description == 'moderate rain'){
        //console.log('rany');
        this.imgPath='rany';
      }else if(this.weather.weather[0].description == 'very heavy rain'){
        //console.log('rany');
        this.imgPath='very';
      }else if(this.weather.weather[0].description == 'few clouds'){
        //console.log('rany');
        this.imgPath='few';
      }
      else{
         //console.log('nothing');
        this.imgPath= ' ';
      }
    }

   }
  }

</script>

<style>

 *{
  margin:0;
  padding:0;
  box-sizing: border-box;
}

body{
  font-family: 'montserrat',sans-serif;
}

#app{
  background-image: url('./assets/fir.jpg') ;
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.rain{
  background-image: url('./assets/rain.jpg');
}

#app.wind{
  background-image: url('./assets/wind.jpg');
}

#app.cloud{
  background-image: url('./assets/cloud.jpg');
}

#app.sun{
  background-image: url('./assets/sun.jpg');
}

#app.few{
  background-image: url('./assets/few.jpg');
}

#app.broke{
  background-image: url('./assets/broke.jpg');
}

#app.scat{
  background-image: url('./assets/scat.jpg');
}

#app.rany{
  background-image: url('./assets/rany.jpg');
}

#app.heavy{
  background-image: url('./assets/heavy.jpg');
}

#app.very{
  background-image: url('./assets/very.jpg');
}

main{
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom,rgba(0,0,0,0.25),rgba(0,0,0,0.75));
}

.search-box{
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar{
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 16px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus{
  box-shadow: 0px 0px 16px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.75);
  border-radius: 16px 0px 16px 0px ;
}

.location-box .location{
  color: #FFF;
  font-size: 40px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0,0.25);
}

.location-box .date{
  color: rgba(255, 255, 255, 0.781);
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
  text-shadow: 1px 1px rgba(0,0,0,0.25);
  
}

.weather-box{
text-align: center;

}

.weather-box .temp{
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 5px rgba(0,0,0,0.25);
}

.weather-box .pres{
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 30px;
  font-weight: 700;
  

  text-shadow: 3px 6px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.25);
  border-radius: 10px;
  margin: 10px 0px 10px 10px;

  box-shadow: 3px 5px rgba(0,0,0,0.25);
}

.weather-box .hum{
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 30px;
  font-weight: 700;

  text-shadow: 3px 6px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.25);
  border-radius: 10px;
  margin: 10px 0px 10px 20px;

  box-shadow: 3px 5px rgba(0,0,0,0.25);
}

.weather-box .pressure{
  display: inline-block;
  color: #FFF;
  font-size: 20px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
   margin: 0px 20px 10px 40px;
}

.weather-box .weather{
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
}

</style>
