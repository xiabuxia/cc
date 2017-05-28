<template>
  <div id="app">
    <div class="event">
      <span class="clock">
        <span class="line-second"></span>
      </span>
      <span>To Do</span>
    </div>
    <div class="input-ct">
      <input type="text" v-model="msg" class="input" @keydown.enter="submit">
      <button class="submit" @click.prevent="submit">提交</button>
    </div>
    <div class="btn-ct" @click="changeBg($event)">
      <button class="btns all" :class="show[0]">全部</button>
      <button class="btns finish" :class="show[1]">已完成</button>
      <button class="btns unfinish" :class="show[2]">未完成</button>
    </div>
    <div class="data-ct all"  v-show="tab[0]">
      <div class="data" v-show="mydata.length===0">暂无数据...</div>
      <ul class="event-ct">
        <li v-for="(item,index) in mydata" v-bind:title="item.content">
          <input type="checkbox" class="checkbox" @click="chioce(index)"  v-model="state[index]" >
          <span ref="text">{{item.content}}</span>
          <button class="delete" @click="cut(index)">删除</button>
        </li>
      </ul>
    </div>
    <Finished v-show="tab[1]" :datas="finished" v-on:newEvent="shell" v-on:removeEvent="remove1"></Finished>
    <Unfinish v-show="tab[2]" :datas="unfinish" v-on:refinish="refinish" v-on:fromUnfinish="remove2"></Unfinish>
    
  </div>
</template>

<script>
import Hello from './components/Hello'
import Finished from './components/Finished'
import Unfinish from './components/Unfinish'

export default {
  name: 'app',
  data: function(){
    return {
      msg:'',
      state: [],
      active: [],
      mydata:[],
      show: ['btn-bg','',''],
      tab:[true,false,false],
      finished:[],
      unfinish:[]
    }
  },
  components: {
    Hello,
    Finished,
    Unfinish
  },
  methods: {
    shell:function(event){
      this.unfinish.push(event[0])
      for(var i=0;i<this.mydata.length;i++){
        if(this.mydata[i].content === event[0].content){
          this.mydata[i].state=false
          this.state[i]=false
          this.$refs.text[i].setAttribute('class','btn-bg')
        }
      }
    },
    remove1: function(arr){

      for(var i=0;i<this.mydata.length;i++){
        if(this.mydata[i].content === arr[0].content){
          this.mydata.splice(i,1)
        }
      }
    },
    remove2: function(arr){

      for(var i=0;i<this.mydata.length;i++){
        if(this.mydata[i].content === arr[0].content){
          this.mydata.splice(i,1)
        }
      }
    },
    refinish: function(arr){
      this.finished.push(arr[0])
      for(var i=0;i<this.mydata.length;i++){
        if(this.mydata[i].content === arr[0].content){
           this.state[i]=true
          this.$refs.text[i].setAttribute('class','active')
        }
      }
    },
    chioce:function(index){
      var num = index
      var node = this.$refs.text[num]
      if(this.state[num]===true){
        this.$nextTick(()=>{
         node.setAttribute('class','active')
        })
        this.mydata[num].state = true
        this.add()
      }else if(this.state[num]===false){
        node.setAttribute('class','')
        this.mydata[num].state = false  
        this.add()  
      }
    },
    submit: function(){
      this.flag = true
      if(this.msg!==''){
        this.mydata.push({'state':false,'content':this.msg})
        this.msg = ''
        this.add() 
      }      
    },
    cut: function(index){
        this.mydata.splice(index,1)
        this.add()
      },
    changeBg: function(event){
      var node = event.target
     if(node.innerText === '全部'){
        this.show = []
        this.show[0] = 'btn-bg'
        this.tab = [true,false,false]
     }else if(node.innerText ==='已完成') {
        this.show = []
        this.show[1] = 'btn-bg'
        this.tab = [false,true,false]
     }else if(node.innerText === '未完成') {
        this.show = []
        this.show[2] = 'btn-bg'
        this.tab = [false,false,true]
     }
    },
    add: function(){
          this.finished = []
          this.unfinish = []
          for(var i=0;i<this.mydata.length;i++){
              if(this.mydata[i].state===true){
                  this.finished.push(this.mydata[i])
              }else if(this.mydata[i].state===false){
                  this.unfinish.push(this.mydata[i])
              }
          }
      },
      init: function(){
        for(var i=0;i<this.mydata.length;i++){
          this.state[i] = false
        }
      }
     
  },
  watch:function(){

  },
  created: function(){
    this.init()
  }

}
</script>

<style>
ul,li {
  list-style: none;
  margin: 0;
  padding: 0;
}
body {
  background: #000;
}
#app {
  font-size: 20px;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  width: 60%;
  color: #2c3e50;
  margin: 60px auto;
  
}
.data {
  font-size: 16px;
  width: 100%;
  margin-top: 10px;
  text-align: center;
  color: #fff;
}
#app .event {
  color: #fff;
  margin-bottom: 10px;
}
#app .event .clock {
  display: inline-block;
  width: 15px;
  height: 15px;
  border: 2px solid #fff;
  border-radius: 50%;
  position: relative;
}
.line-second {
  display: inline-block;
  width: 6px;
  height: 7px;
  border-bottom: 2px solid #fff;
  border-left: 1px solid #fff;
  position: absolute;
  top: 1px;
  left: 6px;
}
.input-ct {
  display: flex;
}
#app .input {
  flex:1;
  height: 40px;
  border-radius: 5px;
  font-size: 16px;
  text-indent: 10px;
  vertical-align: top;
  outline: none;
}
#app .submit {
  flex: 0 0 80px;
  width: 80px;
  height: 46px;
  font-size: 16px;
  margin-left: 10px;
  border: none;
  border-radius: 5px;
  outline: none;
  background: #0ce1f9;
  cursor: pointer;
  
}
#app .submit:active {
  background: #b3f320;
}
.btn-ct {
  display: flex;
  width: 50%;
}
#app .btns {
  flex: 0 0 60px;
  margin-top: 10px;
  margin-right: 10px;
  height: 35px;
  border: none;
  border-radius: 3px;
  outline: none;
  cursor: pointer; 
  color: #fff;
}
#app .btn-ct .btn-bg {
  background: #27555a;
}
#app .all {
  background: #252b24;
}
#app .finish {
  background: #252b24;
}
#app .unfinish {
  background: #252b24;
}
input[type=checkbox] {
  vertical-align: middle;
  margin-right: 10px;
  cursor: pointer;
}
.event-ct {
  margin-top: 10px;
  font-size: 16px;
  color: #fff;
  width: 100%;
}
.event-ct li {
  margin-top: 10px;
  width: 100%;
  display: flex;
  flex-wrap: nowrap;
}
.event-ct li .checkbox {
  flex: 0 0 20px;
}
.event-ct li span {
  flex: 1;
  vertical-align: top;
  flex-wrap: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
.event-ct .delete {
  flex: 0 0 50px;
  border-radius: 3px;
  outline: none;
}
.active {
  text-decoration: line-through;
}
</style>
