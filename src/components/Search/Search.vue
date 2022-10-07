<template>
    <div id="search_container">
        <el-input class="search_input" type="text" placeholder='请速入你要查找的位置' v-model='inputObject.userInput' :id="inputObject.inputId" ></el-input>
        <el-button id="searchBtn" type="primary" @click="send">search</el-button>
    </div>
</template>
<script>
    import bus from '@/eventBus/eventBus.js'
    export default {
        data(){
            return{
               inputObject:{
                userInput:'',
                inputId: "searchInput"
               }
            }
        },
        methods:{
            send(){
                bus.$emit('shareUserInput',this.inputObject)
              }
        },
        mounted(){
            this.send()
        },
    }

</script>
<style scoped>
    #search_container{
        width: 100%;
        width: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
    }
   .search_input{
        height: 30px;
        width:300px;

    }
    #searchBtn{
        height: 35px;
        width: 120px;
        /* background-color: red; */
    }
</style>