<template>
    <div id="index_container ">
        <div id="map_warp">
            <MapContaner></MapContaner>
        </div>
    </div>

  </template>
  
  <script>
import MapContaner from '../components/MapContainer/MapContaner.vue';
  export default {
    components: { MapContaner }
}
  </script>
  
  <style lang="css" scoped>
    #index_container{
        width: 100%;
        height: 100%;
        position: relative;
    }
    #map_warp{
        width: 60%;
        height: 60%;
    }
  </style>
  