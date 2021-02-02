<template>
  <div class="userlist">
      <h1>选择登陆</h1>
      <div class="list">
          <div class='item' v-for="item,index in userlist" :key="'userlist'+index" @click="chooseUser(index,item)">
              <img :src="item.head" alt="">
              <p>{{item.username}}</p>
          </div>
      </div>
  </div>
</template>

<script>
import socket from './socket';
export default {
    name:'userlist',
    props:['userlist'],
    data(){
        return{

        }
    },
    methods:{
        chooseUser:function(index,item){
            this.$root.me = item
            this.$emit('choose',index)
            console.log('choose user')
            console.log(this.$root.me)
            // localStorage.chatme = JSON.stringify(item)
            socket.emit('login',item)
        }
    }
}

</script>
<style scoped>
h1{
    color: rebeccapurple;
}
.userlist{
    width: 300px;
    height: 600px;
    background: skyblue;
    display: flex;
    /* flex-wrap: wrap; */
    flex-direction: column;
}
.list{
    width: 100%;
    align-self: flex-start;
    display: flex;
    flex-wrap: wrap;
    padding-left: 10px;
}
.item{
    width: 50%;
    list-style: none;
    display: flex;
}
.userlist .list .item img{
    height: 45px;
    width: 45px;
    border-radius: 6px;
}
img{
    height: 45px;
    width: 45px;
    border-radius: 6px;
}
</style>