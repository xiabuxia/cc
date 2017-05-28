<template>
    <div class="unfinishe">
        <div class="data" v-show="datas.length===0">暂无数据...</div>
        <ul class="event-ct">
                <li v-for="(item,index) in datas"  v-bind:title="item.content">
                    <input type="checkbox" class="checkbox" checked='false' v-model="stateArr[index]" @click="finish">
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
                unfinish: [],
                stateArr: []
            }
        },
        watch: {
            datas:function(){
                this.initState()
            }
        },
        methods: {
            initState(){
                for(var i=0;i<this.datas.length;i++){
                    this.stateArr[i] = false
                }
            },
            finish(){
                for(var i=0;i<this.datas.length;i++){
                    if(this.stateArr[i]===true){
                        this.unfinish = this.datas.splice(i,1)
                        this.unfinish[0].state=true
                        this.$emit('refinish',this.unfinish)
                    }
                }
                this.initState()
            },
            remove(index){
                this.unfinish = this.datas.splice(index,1)
                this.$emit('fromUnfinish',this.unfinish)

            }
        }
    }
    
</script>
<style>
    
</style>