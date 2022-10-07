<template>
    <div id="info_window_container">
        <div>信息展示</div>
        <div>
            <div>地址</div>
            <span>{{placeAddress}}</span>
        </div>
        <div>
            <div>坐标</div>
            <span>{{placeLocation}}</span>
        </div>
    </div>
</template>
<script>
       import bus from '@/eventBus/eventBus.js'
 export default{
    data(){
        return{
            placeAddress:"",
            placeLocation:''
        }
    },
    created(){
        bus.$on('shareAddressDetail',(val)=>{
            console.log(val);
            this.placeAddress = val.address,
            this.placeLocation = val.pos
        })
    }
 }
</script>
<style>
#info_window_container{
    height: 150px;
    width: 220px;
    background-color: rgba(223,223,223);
}
</style>