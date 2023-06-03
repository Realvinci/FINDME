<template>
    <div>
      <l-map style="height: 100vh" :zoom="zoom" :center="center">
      <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
      <l-marker :lat-lng="markerLatLng"></l-marker>
     </l-map>
    </div>

</template>
  
  <script>
  import {LMap, LTileLayer, LMarker} from 'vue2-leaflet';
  import apikey from '@/views/env.js'
  export default {
    components: {
      LMap,
      LTileLayer,
      LMarker
    },
    data () {
      return {
        url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
        attribution:
          '&copy; <a target="_blank" href="http://osm.org/copyright">OpenStreetMap</a> contributors',
        zoom: 15,
        center: [9.8965, 8.8583],
        markerLatLng: [9.8965, 8.8583],
        city:'abuja',
        country:'nigeria'
      };
    },
    methods:{
      locatorButtonPressed() {
      navigator.geolocation.getCurrentPosition(
       position => {
       console.log(position.coords.latitude);
       this.markerLatLng[0] = position.coords.latitude
       console.log(position.coords.longitude);
       this.markerLatLng[1] = position.coords.longitude
       this.center[0] = [this.markerLatLng[0]]
       this.center[1] = [this.markerLatLng[1]]
     },
     error => {
       console.log(error.message);
     },
  )   
   },
    async getLatitudeandLongitude(){
      const res = await fetch(`https://app.zenserp.com/api/v2/search?apikey=4b35a760-5c59-11ed-89fc-991a6bcfe09f&q=${this.city}%20${this.country}&tbm=lcl`).then(res=>res.json())
      console.log(res.maps_results[0].coordinates);
      
    }
    },
    created(){
      this.locatorButtonPressed();
      console.log(this.markerLatLng)
    //  this.getLatitudeandLongitude()
    }
  }
  </script>
  
  <style>
  
  </style>
  