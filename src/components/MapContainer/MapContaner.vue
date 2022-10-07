<template>
    <div id="container"></div>
  </template>
  
  <script>
    import bus from '@/eventBus/eventBus.js'
  import AMapLoader from '@amap/amap-jsapi-loader'
      window._AMapSecurityConfig = {
            securityJsCode: '9f0fdc7fe347d482db8a3cc4e7b6320b'
      }
  export default {
    data() {
      return {
        map: null,
        autoOptions: {
        input: ''
        },
        searchPlaceInput:'',
        auto: null,
        placeSearch:null

      }
    },
    methods: {
      select(e) {
      this.placeSearch.setCity(e.poi.adcode)
      this.placeSearch.search(e.poi.name) //关键字查询查询
      // this.map.setZoom(10,true)
    },
      initMap() {
        AMapLoader.load({
          key: 'da24dfc6ed8d4233dcddcd5ba8026a0e', // 申请好的Web端开发者Key，首次调用 load 时必填
          version: '2.0', // 指定要加载的 JSAPI 的版本，缺省时默认为 1.4.15
          plugins: ['AMap.ToolBar', 'AMap.Scale', 'AMap.HawkEye', 'AMap.MapType', 'AMap.Geolocation,','AMap.AutoComplete','AMap.PlaceSearch'] // 需要使用的的插件列表，如比例尺'AMap.Scale'等
        })
          .then(AMap => {
            this.map = new AMap.Map('container', {
              //设置地图容器id
              viewMode: '3D', //是否为3D地图模式
              zoom: 18, //初始化地图级别
              center: [104.154434,30.681506] //初始化地图中心点位置
            })
            this.map.addControl(new AMap.Scale())
            this.map.addControl(new AMap.ToolBar())
          this.map.addControl(new AMap.HawkEye())
          this.map.addControl(new AMap.MapType())
          this.map.addControl(new AMap.Geolocation())
          this.auto = new AMap.AutoComplete(this.autoOptions)
          this.placeSearch = new AMap.PlaceSearch({
            map: this.map
          }) //构造地点查询类
          this.auto.on('select', this.select)
          })
          .catch(e => {
            console.log(e)
          })
      }
    },
    mounted() {
      //DOM初始化完成进行地图初始化
      this.initMap()
    },
    created() {
    bus.$on('shareUserInput', val => {
      this.autoOptions.input = val.inputId
      this.searchPlaceInput = val.userInput   
    })
  },
  watch: {
    searchPlaceInput(newValue) {
      if (newValue != null) {
        this.placeSearch.search(newValue)
      }
    }
  }


  }
  </script>
  
  <style lang="css">
  #container {
    padding: 0px;
    margin: 0px;
    background-color: red;
    width: 60vw;
    height: 60vh
  }
  </style>
  
  