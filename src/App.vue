<template>
  <div id="app" :class="{'light': !isDark}">
    <the-header @get-city="searchCity= $event" @get-theme="isDark= !isDark" :isDark="isDark"></the-header>
    <transition-group name="fade">

      <div class="loading" v-if="isLoading">
          <img src="./assets/images/Dual-Ball-1s-200px.svg" alt="loading ball">
      </div>
      <the-weather v-else
        :weatherData="forecastData" 
        :location="fcLocation" 
        @getDay='day= $event' 
        :error='error'
        :loading='isLoading'
      ></the-weather>
    </transition-group>
    
  </div>
</template>
<script>
import theHeader from './components/theHeader'
import theWeather from './components/theWeatherBox'
import {reactive, ref, computed, watch } from 'vue'
import axios from 'axios'
export default{
  components: {
    theHeader,
    theWeather
    
  },
  setup(){

    const isDark= ref(true)
    const error= ref(null)
    const isLoading= ref(false)
    //location and days variables/////////////////////////////
    const day= ref(0)
    const latLocation= ref('')
    const lonLocation= ref('')
    const searchCity= ref('')
    const location= computed(()=>{
      if(searchCity.value !== ''){
        return searchCity.value
      }
      return `${latLocation.value},${lonLocation.value}`

    })
    //weather data////////////////////////////////////////////
    const forecast= ref(null)
    
    const forecastData= computed(()=>{
      return forecast.value[day.value]
    })
    const fcLocation= ref(null) 
/////////////////////////////////////////////////////////////////////////////////////////
    //get the location******************************************************************
////////////////////////////////////////////////////////////////////////////////////////
    async function getLocation(){
        const locationData = await axios.get(`http://ip-api.com/json/`)
        //error.value= null
        latLocation.value= locationData.data.lat
        lonLocation.value= locationData.data.lon
    } 

    //get the weather**********************************************************
    async function getWeather(){
      isLoading.value=true
      try {
        await getLocation()
        const weatherData = await axios.get(`http://api.weatherapi.com/v1/forecast.json?key=545aad964706421ca0d163403202511&q=${location.value}&days=3`)
        isLoading.value= false
        error.value= null
        forecast.value= weatherData.data.forecast.forecastday
        fcLocation.value= weatherData.data.location
      } catch (err) {
        isLoading.value= false
       if (err.response) {
      
      
        error.value= err.response.data.error.message
      } else if (err.request) {
      
        console.log(err.request);
      } else {
        error.value=error.message
      }
    }
      
    }
    watch(location, (newVal, oldVal)=>{
      getWeather()
    })
    
    getWeather()


    return{
      latLocation,
      lonLocation,
      forecast,
      forecastData,
      fcLocation,
      day,
      searchCity,
      error,
      isDark,
      isLoading
    }
  }
}

</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Lato:wght@300;400;700&display=swap');
$bp-largest: 75em;   // 1200px
$bp-large: 62.5em;   // 1000px
$bp-medium: 50em;    // 800px;
$bp-small: 37.5em;    // 600px;
$bp-smallest: 25em;   //400px;
:root{
  --primary-color: #212730;
  --primary-color-2: #393e46;
  --secondary-color: #ffc93c;
  --light-color: #eeeeee;
  --error-color: #f05454;
}
::selection{
  background-color: var(--secondary-color);
}
*, *:after, *:before{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
html{
  font-size: 62.5%;
  @media only screen and (max-width: $bp-large) {
    font-size: 56.25%;
  }
  @media only screen and (max-width: $bp-medium) {
    font-size: 50%;
  }
  @media only screen and (max-width: $bp-small) {
    font-size: 43.75%;
  }
}
body{
  font-family: 'Lato', sans-serif;
  font-size: 2rem;
  font-weight: 400;
  
}
#app{
  color: var(--light-color);
  background-color: var(--primary-color);
  transition: .2s ease;
  min-height: 100vh;
  
}
#app.light{
    --light-color: rgb(33, 39, 48);
    --primary-color-2: #e2e2e2;
    --primary-color: #f7f7f7;
  }
.container{
  max-width: 104rem;
  margin: 0 auto;
  @media only screen and (max-width: $bp-large) {
      max-width: 90rem;
  }
  @media only screen and (max-width: $bp-medium) {
      max-width: 75rem;
  }
  @media only screen and (max-width: $bp-small) {
      max-width: 55rem;
  }
  @media only screen and (max-width: $bp-smallest) {
      max-width: 45rem;
  }
}
.loading{
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
