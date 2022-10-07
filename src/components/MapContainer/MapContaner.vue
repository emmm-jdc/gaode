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
        placeSearch:null,
        district: null,
      polygons: [],
      showHeatOrNot:false,
      heatMap:null,
      heatmapList: []

      }
    },
    methods: {
      FeedBack(msg,feedbackType) {
        this.$message({
          showClose: true,
          message: msg,
          type: feedbackType
        });
      },
      select(e) {
      this.placeSearch.setCity(e.poi.adcode)
      this.placeSearch.search(e.poi.name) //关键字查询查询    
      // this.map.setZoom(10,true)
      console.log(e.poi.name);

      this.drawBounds(e.poi.name)
        this.map.setZoom(16, true, 1)
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
      },
      // 行政区区域划分
    drawBounds(newValue) {
      //加载行政区划插件
      if (!this.district) {
        //实例化DistrictSearch
        var opts = {
          subdistrict: 0, //获取边界不需要返回下级行政区
          extensions: 'all', //返回行政区边界坐标组等具体信息
          level: 'district' //查询行政级别为 市
        }

        this.map.plugin(['AMap.DistrictSearch'], () => {
          this.district = new AMap.DistrictSearch(opts)
        })
        // this.district = new AMap.DistrictSearch(opts)
      }
      //行政区查询
      this.district.search(newValue, (status, result) => {
        if(result != null){
          this.FeedBack('区域搜索成功',"success")
        }else{
          this.FeedBack('区域搜索失败',"error")
        }
        if (this.polygons != null) {
          this.map.remove(this.polygons) //清除上次结果
          this.polygons = []
        }
        var bounds = result.districtList[0].boundaries
        if (bounds) {
          for (var i = 0, l = bounds.length; i < l; i++) {
            //生成行政区划polygon
            var polygon = new AMap.Polygon({
              strokeWeight: 1,
              path: bounds[i],
              fillOpacity: 0.4,
              fillColor: '#80d8ff',
              strokeColor: '#0091ea'
            })
            this.polygons.push(polygon)
          }
        }
        this.map.add(this.polygons)
        this.map.setFitView(this.polygons) //视口自适应
      })
    },
    showHeateMap(){
      this.map.plugin(['AMap.PlaceSearch'], () => {
        //构造地点查询类
        var placeSearch = new AMap.PlaceSearch({
          pageSize: 50, // 单页显示结果条数
          pageIndex: 1, // 页码
          city: this.searchPlaceInput, // 兴趣点城市
          citylimit: true //是否强制限制在设置的城市内搜索
          //map: this.map, // 展现结果的地图实例
          // panel: 'panel', // 结果列表将在此容器中进行展示。
          // autoFitView: true // 是否自动调整地图视野使绘制的 Marker点都处于视口的可见范围
        })
        //关键字查询
        placeSearch.search('-sc-', (status, result) => {
          // console.log(result)

          this.getHotChartPos('-sc-', result)
        })
      })
      this.$notify({
        title: '成功',
        message: '热力图获取成果，但是由于电脑性能，我们仅加载部分数据',
        type: 'success'
      })
    },
    getHotChartPos(detail, result) {
      let lengthSize = result.poiList.pageSize
      const count = result.poiList.count
      // const lengthPage = count / lengthSize
      if (lengthSize > count) {
        lengthSize = count
      }
      for (var n = 0; n < 2 ; n++) {
        // this.map.plugin(['AMap.PlaceSearch'], () => {
        //构造地点查询类
        var realSearch = new AMap.PlaceSearch({
          pageSize: 50, // 单页显示结果条数
          pageIndex: n + 1, // 页码
          city: this.searchPlaceInput, // 兴趣点城市
          citylimit: true //是否强制限制在设置的城市内搜索
          // map: this.map, // 展现结果的地图实例
          // panel: 'panel', // 结果列表将在此容器中进行展示。
          // autoFitView: true // 是否自动调整地图视野使绘制的 Marker点都处于视口的可见范围
        })
        realSearch.search(detail, (status, result) => {
          // for (var j = 0; j < 50; j++) {
          // this.map.remove(this.map.getAllOverlays('marker'))
          //var centerPoint = [result.poiList.pois[j].location.lng, result.poiList.pois[j].location.lat]
          // console.log(result)
          //热力图
          this.showHatChart(result)
          // }
        })
      }
    },
    showHatChart(result) {
      var info = []
      for (let i = 0; i < 50; i++) {
        info.push({
          lng: result.poiList.pois[i].location.lng,
          lat: result.poiList.pois[i].location.lat,
          count: 3 * 6.4 * Math.round(Math.random() * (10 - 2) + 2)
        })
      }

      this.map.plugin(['AMap.HeatMap'], () => {
        // console.log('nn')
        //初始化heatmap对象
        this.heatmap = new AMap.HeatMap(this.map, {
          radius: 56, //给定半径
          opacity: [0, 0.5]
          /*,
            gradient:{
                0.5: 'blue',
                0.65: 'rgb(117,211,248)',
                0.7: 'rgb(0, 255, 0)',
                0.9: '#ffea00',
                1.0: 'red'
            }
             */
        })
        //设置数据集
        this.heatmap.setDataSet({
          data: info,
          max: 100
        })
        this.heatmapList.push(this.heatmap)
        this.heatmap.show()
      })
    },

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
    bus.$on('shareHeatMapShow', val => {
      this.showHeatOrNot = val
    })
  },
  watch: {
    searchPlaceInput(newValue) {
      if (newValue != null) {
        this.placeSearch.search(newValue)
        this.drawBounds(newValue)
        this.map.setZoom(16, true, 1)
      }
    },
    showHeatOrNot(newValue) {
      // console.log(newValue);
      if (newValue) {
        this.showHeateMap()
      }else{
        this.heatMap.hide()
      }
    },
  }


  }
  </script>
  
  <style lang="css">
  #container {
    padding: 0px;
    margin: 0px;
    /* background-color: red; */
    width: 60vw;
    height: 60vh
  }
  </style>
  
  