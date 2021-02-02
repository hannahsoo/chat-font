<template>
  <div id="app">
    <!-- <img src="./assets/logo.png"> -->
    <router-view/>
    <div class="content">
      <chat-com class="chat" :touser="touser" :msgList="msgList"
            ></chat-com>
      <userlist :userlist="usrlist"
                v-if="$root.me==null"
                @choose='toggleUser'></userlist>
      <onlineList v-if="$root.me!=null" :head="$root.me.head" 
      :islogin="islogin" :users="users" :chooseUser="chooseUser"></onlineList>
    </div>
    
  </div>
</template>

<script>
import chatCom from "./components/chat-com"
import userlist from "./components/userlist"
import axios from "axios"
import onlineList from "./components/onlinelist"
import io from 'socket.io-client'
import socket from './components/socket'

export default {
  name: 'App',
  components:{
    chatCom,
    userlist,
    onlineList,
  },
  async beforeMount(){
    // axios.get('http://localhost:3000/api/userlist').then(function(res){
    //   console.log(res)
    // })
    let res = await axios.get('http://localhost:3000/api/userlist')
    this.usrlist = res.data
  },
  mounted(){
    socket.on('login',(data)=>{
      if(data.state=='OK'){
        this.islogin = true  
      }
    })
    socket.on('logout',(data)=>{
      console.log('您已下线')
      console.log(data.content)
      this.islogin = false
      socket.disconnect()
      this.$root.me=null
    })
    socket.on('disconnect',(data)=>{
      console.log('断开连接')
    })
    socket.on('users',(data)=>{
      // console.log('userlist pulled')
      this.users = data
      // console.log(data)
      if(JSON.stringify(this.touser)=='{}'){
        // console.log('{}')
      }else{
        this.touser = this.usrlist[this.touser.id - 1]
      }
    })
  },
  data(){
    return{
      msg:'hello xxh',
      usrlist:[],
      // currentUser:{
      //     username:'zhouyin',
      //     head:require('./assets/head/headzy.png')
      // },
      me:null,
      islogin:false,
      users:[],
      touser:{},
      msgList:[]
    }
  },
  methods:{
    toggleUser:async function(index){
      let res = await axios.get('http://localhost:3000/api/userlist')
      this.usrlist = res.data
      // this.currentUser = this.usrlist[index]
    },
    chooseUser:function(user){
      this.touser = user
      let strKey = 'chat-'+this.$root.me.username+'-'+this.touser.username
      console.log(JSON.parse(localStorage[strKey]))
      this.msgList = JSON.parse(localStorage[strKey])
    }
  }
}
</script>

<style scoped>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.content{
  display: flex;
  width: 800px;
  height: 600px;
  margin: 0 auto;
  border: 2px solid rgb(113, 163, 219);
  box-shadow: 0 4px 4px 0 #7f7f7f;
}
.chat{
  width: 100%;
}
</style>
