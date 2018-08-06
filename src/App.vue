<template>
    <div style="text-align: center; padding-top: 20px; background-color: #1776c1; height:100vh;">
      <img :src=iconUrl alt="weather icon" height="100" width="100">
      <h1 style="padding-top: 20px;">{{temp}}℃</h1>
      <h3 style="padding-top: 20px;">{{weatherMain}}</h3>
      <h3 style="padding-top: 20px;">{{suggestion}}</h3>
      <h3 style="padding-top: 20px;">{{locationName}}</h3>
    </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      lat: '',
      lon: '',
      temp: '',
      weatherMain:'',
      locationName: '',
      iconUrl: '',
      suggestion: ''
    }
  },
  mounted() {
    this.getLocation()
  },
  methods: {
    getLocation: function () {		
	    if(navigator.geolocation){
	      navigator.geolocation.getCurrentPosition(this.showPosition)
	    }else{
	      this.error = "Geolocation is not supported."
      }
    },
	  showPosition: function (position) {	
      this.lat = position.coords.latitude
      this.lon = position.coords.longitude
      axios.get(`http://api.openweathermap.org/data/2.5/weather?appid=87ed06b419c8d79443828bce820adead&lat=` + this.lat + `&lon=` + this.lon + `&units=metric`)
      .then( res => {
        this.locationName = res.data.name
        this.temp = res.data.main.temp
        this.weatherMain = res.data.weather[0].main + ', ' + res.data.weather[0].description
        this.iconUrl =  'http://openweathermap.org/img/w/' + res.data.weather[0].icon + '.png'
        if (this.temp >= 23) {
          this.suggestion = 'It is a hot day'
        } else if (this.temp >= 15 && this.temp < 23) {
          this.suggestion = 'Long-sleeved clothes'
        } else if (this.temp >= 0 && this.temp < 15) {
          this.suggestion = 'Sweater is best'
        } else {
          this.suggestion = 'Too cold, warmer dress'
        }
      })
	  },
  }
}

</script>
<style scoped>
h1{color: white;}
h3{color: white;}
</style>
