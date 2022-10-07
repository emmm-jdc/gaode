<template>
    <div id="index_container ">
      <div id="search_wrap">
        <search></search>
      </div>
        <div id="map_warp">
            <MapContaner></MapContaner>
        </div>
    </div>

  </template>
  
  <script>
import MapContaner from '../components/MapContainer/MapContaner.vue';
import Search from "@/components/Search/Search.vue"
    export default {
    components: { MapContaner , Search}
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
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%,-50%);
    }
    #search_wrap{
      position: absolute;
      top: 10%;
      left: 40%;
      height: 60px;
      width: 400px;
    }
  </style>
  