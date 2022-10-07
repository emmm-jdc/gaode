<template>
        <div id="heat_map_search_box">
            <!-- <el-radio v-model="radio" label="1">热力图</el-radio> -->
            <el-checkbox v-model="checked" @change="send">热力图</el-checkbox>
         </div>
</template>
<script>
        import bus from '@/eventBus/eventBus.js'
    export default{
        data(){
            return{
                checked:false
            }
        },
        methods:{
            send(){
                bus.$emit('shareHeatMapShow',this.checked)
            }
        }
    }
</script>
<style scoped>
    #heat_map_search_box{
        height: 100%;
        width: 100%;
        position: relative;

    }
</style>