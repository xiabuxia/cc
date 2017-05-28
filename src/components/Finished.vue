<template>
    <div class="finished">
        <div class="data" v-show="datas.length===0">暂无数据...</div>
        <ul class="event-ct">
            <li v-for="(item,index) in datas"  v-bind:title="item.content">
                <input type="checkbox" class="checkbox" checked=true v-model="stateArr[index]" @click="cut(index)">
                <span>{{item.content}}</span>
                <button class="delete" @click="remove(index)">删除</button>
            </li>
        </ul>
    </div>
</template>
<script>
    export default {
        props:{
            datas:Array
        },
            
        data: function(){
            return {
                finished: [],
                stateArr: []
            }
        },
        watch: {
            datas:function(){
                this.initState()
            }
        },
        methods: {
            cut(index){
                for(var i=0;i<this.stateArr.length;i++){
                    if(this.stateArr[i]===false){
                        this.finished = this.datas.splice(index,1)
                        this.finished[0].state =  false
                    }
                }
                this.$emit('newEvent',this.finished)
            },
            initState(){
                for(var i=0;i<this.datas.length;i++){
                    this.stateArr[i] = true
                }
            },
            remove: function(index){
                this.finished = this.datas.splice(index,1)
                this.$emit('removeEvent',this.finished)
            }
        }
    
        
    }
</script>
<style>
    
</style>