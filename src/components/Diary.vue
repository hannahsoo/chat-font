<template>
  <div>
      <button @click="getDiary">进入日记</button></div>
</template>

<script>
import axios from "axios"
export default {
    methods:{
        getDiary:async function(){
            let res = await axios.get('https://docs.qq.com/desktop/mydoc')
            console.log(res.data)
        }
    }
}

</script>
<style scoped>
</style>