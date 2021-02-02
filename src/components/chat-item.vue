<template>
  <div>
    
    <div class="msg">
      <img :src="msginfo.from.head" alt="">
      <div class="message">
        <div class="arrow"></div>
        <div class="msg-text">
            <!-- <slot></slot> -->
            {{msginfo.content}}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
    props:['msginfo']
}

</script>
<style scoped>
.msg .message::before{
    display: block;
    content: "";
    width: 0;
    height: 0;
    border: solid 6px ;
    position: absolute;
    top:50%; 
    margin-top: -7px;
    border-color: #efefef greenyellow #efefef #efefef;
}
.message{
    position: relative;
}
.msg{
    /* align-self: flex-start; */
    /* float: left; */
    /* width: 100%; */
    display: flex;
    margin: 3px 3px;
}
.msg-text{
    background: greenyellow;
    padding: 3px 5px;
    border-radius: 5px;
    margin-left: 12px;
    max-width: 400px;
    white-space: normal;
    word-break: break-all;
    overflow: hidden;
    font-size: 16px;
}
img{
    height: 30px;
    width: 30px;
}
.self .msg{
  display: flex;
  flex-direction: row-reverse;
}
.self .msg .message{
  margin-right: 12px;
}
.self .msg .message::before{
  display: none;
}
.self .msg .message .arrow{
  width: 0;
  height: 0;
  border: solid 6px ;
  position: absolute;
  top:50%; 
  right: -12px;
  margin-top: -7px;
  border-color: #efefef  #efefef #efefef greenyellow;
}
</style>