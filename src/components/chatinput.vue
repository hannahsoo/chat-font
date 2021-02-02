<template>
  <div class="input-com">
      <input type="text" @keydown.enter="send" v-model="inputContent">
      <button @click="send">send</button>
  </div>
</template>

<script>
export default {
    props:['sendMsg'],
    data(){
        return{
            inputContent:''
        }
    },
    methods:{
        send:function(){
            this.sendMsg(this.inputContent)
            this.inputContent = ''
        }
    }
}

</script>
<style scoped>
.input-com{
    height: 42px;
    display: flex;
    justify-content: space-between;
    padding: 5px 10px;
}
.input-com input{
    flex: 1;
    margin-right: 20px; 
    border-radius: 5px;
    border-color: rgb(113, 163, 219);
    font-size: 18px;
    outline: none;
}
.input-com input:focus{
    border-radius: 5px;
    border:2px solid rgb(113, 163, 219);
}
.input-com button{
    border: 1px #DDD solid;
    border-radius: 15px;
    background: rgb(113, 163, 219);
    font-size: 15px
}
</style>