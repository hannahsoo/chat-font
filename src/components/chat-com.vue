<template>
  <div class="chat">
      <h3 class="user">User: {{touser.username}}</h3>
      <div class="chatlist">
          <chatItem v-for="item,index in msgList" :key="'msg'+index"
                    :msginfo="item" :class="{self:$root.me.username==item.from.username}">
          </chatItem>
      </div>
      <chatinput :sendMsg="sendMsg"></chatinput>
      <div class="btns">
        <!-- <button @click="sendmsgto">sendto</button> -->
        <button @click="joinRoom">groom</button>
        <button @click="sendGroupMsg">群聊</button>
      </div>
  </div>
</template>

<script>
import chatinput from "./chatinput"
import chatItem from "./chat-item"
import "../js/socket.io"
import socket from"./socket"


export default {
    name:'chat',
    props:['touser','msgList'],
    components:{
        chatinput,
        chatItem
    },
    data(){
        return{
            // msgList:[],
        }
    },
    mounted(){
            // console.log(io.connect)
        socket.on('addUser',function(data){
        })
        socket.on('msg',(data)=>{
            var _this = this
            _this.msgList.push(data)
            _this.saveChat()
        })
    },
    beforeMount(){
        // this.getChat()
    },
    updated(){
        let ele = document.getElementById('chatlist')
        ele.scrollTop = ele.scrollHeight
    },
    methods:{
        sendMsg:function(value){
            let msg = {
                from:this.$root.me,
                to:this.touser,
                content:value,
                time:new Date().getTime()
            }
            socket.emit('sendTo',msg)
            // socket.emit('sendMsg')
            this.msgList.push(msg)
            this.saveChat()
        },
        saveChat:function(){
            let strKey = 'chat-'+this.$root.me.username+'-'+this.touser.username
            localStorage[strKey] = JSON.stringify(this.msgList)
        },
        getChat:function(){
            let strKey = 'chat-'+this.$root.me.username+'-'+this.touser.usernamec
            this.msgList = JSON.parse(localStorage[strKey])
        },
        joinRoom:function(){
            console.log('joinroom')
            this.connIns.emit('joinRoom',{room:'groom'})
            
            // this.connIns.emit('sendToRoom',{room:'room1',content:'群聊消息',user:this.$parent.currentUser})
            this.connIns.on('groupChat',function(data){
                console.log('groupchat2')
                console.log(data)
                // console.log(this.msgList)
                // this.sendMsg({user:data.user,msg:data.content})
            })
        },
        sendGroupMsg:function(){
            console.log('sendgroupmsg2')
            this.connIns.emit('sendToRoom',{room:'groom',msg:'群聊消息',user:this.$root.me})
            
        }
    }
}

</script>
<style scoped>
.chat{
    /* width: 500px; */
    height: 600px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    background: #efefef
}
.chatlist{
    width: 100%;
    height: 438px;
    border-top: 1px solid sandybrown;
    border-bottom: 1px solid sandybrown;
    overflow: scroll;
}
.chatlist::-webkit-scrollbar { 
    width: 0 !important; 
    height: 0 !important;}
.input{
    margin-top: 10px;
    height: 50px;
}
.btns{
    margin:10px 0;
}
</style>