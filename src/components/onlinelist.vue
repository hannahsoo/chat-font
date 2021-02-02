<template>
  <div class="online-list">
      
      <div class="me">
          <img :src="head" alt="head" class="headimg" :class="{online:islogin}">
          <div class="messages">消 息 <span v-if="unreadMsg.length>0">({{unreadMsg.length}})</span></div>
          <div class="headimg" @click="quit">退出</div>
      </div>
      <h3>用户列表</h3>
      <div class="list">
          <div class='item' v-for="item,index in friends" :key="'online'+index"
                @click="chooseUser(item)">
              <img :src="item.head" :class="{online:item.isonline=='true'}">
              <div class="right">
                  <p>{{item.username}}</p>
                  <div class="msg"></div>
              </div>              
          </div>
      </div>
  </div>
</template>

<script>
import socket from './socket'
export default {
    props:['head','islogin','users','chooseUser'],
    data(){
        return{
            onlineList:[],
            unreadMsg:[],
            unreadList:[]//小红点用
        }
    },
    mounted(){
        socket.emit('users')
        socket.on('unread',(data)=>{
            var _this = this
            _this.unreadMsg = data
            
            data.forEach(element => {
                _this.unreadList.push(element.fromUser)
            });
            console.log(_this.unreadList)
        })
    },
    methods:{
        quit:function(){
            console.log('quit')
            socket.disconnect()
            this.$root.me = null
            console.log(this.$root)
        }
    },
    computed:{
        friends:function(){//返回一个数组，让v-for进行循环
            return this.users.filter((item,index)=>{
                return item.head!= this.head //this.head是当前用户的属性
            })
        }
    }
}

</script>
<style scoped>
.online-list{
    width: 300px;
    height: 600px;
    background: skyblue;
    display: flex;
    flex-direction: column;
}
.me{
    display: flex;
    flex-direction: row;
    padding: 20px 10px;
    justify-content: space-between;
}
.me img{
    width: 40px;
    height: 40px;
    border-radius: 4px;
}
.messages{
    font-weight: 900;
    font-size: 18px;
}
.list{
    width: 100%;
    align-self: flex-start;
    display: flex;
    flex-wrap: wrap;
    padding-left: 10px;
}
.item{
    width: 100%;
    list-style: none;
    display: flex;
}
.online-list .list .item img{
    height: 45px;
    width: 45px;
    border-radius: 6px;
    filter:grayscale(1);
}
.headimg{
    filter:grayscale(1);
    width: 40px;
    height: 40px;
}
.online{
    filter:grayscale(0) !important;
}
</style>