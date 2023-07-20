<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <el-row>
      <el-col :span="21">
        <el-input type="textarea" v-model="question"></el-input>
      </el-col>
      <el-col :span="3">
        <el-button @click="sendMessage" type="primary">发送</el-button>
      </el-col>
    </el-row>
    <el-row style="margin-top: 10px;">
      <el-col :span="3">输出文字:</el-col>
      <el-col :span="21" >{{text}}</el-col>
    </el-row>
    
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {
  },
  data(){
    return {
      websocketUrl: 'ws://124.222.224.186:8800', // websocket 地址
      socket: null, // Socket对象
      text: '', //websocket回显文字
      question: '' //输入框文字
    }
  },
  mounted(){
    this.initWebSocket()
  },
  methods: {
    // 初始化websocket
    initWebSocket(){
      this.socket = new WebSocket(this.websocketUrl)
      this.socket.onerror = this.websocketErr
      this.socket.onopen = this.websocketOpen
      this.socket.onmessage = this.websocketMessage
      this.socket.onclose = this.websocketClose
      let that = this
			window.onbeforeunload = function() {
				that.websocketClose();
			}
    },
    // websocket打开方法
    websocketOpen(e){
      console.log("webSocket打开",e);
    },
    // webSocket报错方法
    websocketErr(err){
      console.log(err,"webSocket报错");
    },
    // websocket显示信息
    websocketMessage(e){
      let data = e.data
      this.text += data
      this.$forceUpdate()
    },
    // websocket关闭
    websocketClose(close){
      console.log("关闭",close);
      this.socket.close()
    },
    sendMessage(){
      this.socket.send(this.question)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
