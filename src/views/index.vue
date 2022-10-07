<template>
    <div id="index_container ">
      <div id="search_wrap">
        <search></search>
      </div>
        <div id="map_warp">
            <MapContaner></MapContaner>
        </div>
        <div id="heatMap_warp">
          <heatMapSearchBox></heatMapSearchBox>
        </div>
          <div id="info_window_warp">
            <infoWindow></infoWindow>
          </div>
        <div id="shadow"></div>
    </div>

  </template>
  
  <script>
import MapContaner from '@/components/MapContainer/MapContaner.vue';
import Search from "@/components/Search/Search.vue"
import heatMapSearchBox from '@/components/heatMapSearchBox/heatMapSearchBox.vue';
import infoWindow from '../components/infoWindow/infoWindow.vue';
    export default {
    components: { MapContaner , Search,heatMapSearchBox,infoWindow}
}
  </script>
  
  <style lang="css" scoped>
    #index_container{
        background-color: rgb(169, 200, 227);
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
    #heatMap_warp{
      position: absolute;
      top: 16%;
      left: 50%;
      height: 60px;
      width: 400px;
    }
    /* #shadow{
      height: 80%;
      width: 80%;
      position: absolute;
      top: 50%;
    } */
  </style>
  