<template>
  <Navbar />
  <div class="container grid-xl">
    <div class="main">
      <div class="columns">
        <div class="column col-3 col-md-12">
          <Info :data="host" />
        </div>
        <div class="column col-9 col-md-12">
          <div class="columns">
            <div class="column col-6 col-sm-12">
              <CPU :data="cpu" />
            </div>
            <div class="column col-6 col-sm-12">
              <Memory :data="memory" />
            </div>
          </div>
        </div>
      </div>

      <div class="disk">
        <Disk :data="disk" />
      </div>

      <div class="network" v-if="notEmpty(network)">
        <Network :data="network" />
      </div>
    </div>
  </div>
</template>

<script >
import HelloWorld from './components/HelloWorld.vue'
import Navbar from './components/Navbar.vue'
import Info from './components/Info.vue'
import CPU from './components/CPU.vue'
import Memory from './components/Memory.vue'
import Disk from './components/Disk.vue'
import Network from './components/Network.vue'

export default {
  name: "App",
  components: {
    Navbar,
    Info,
    CPU,
    Memory,
    Disk,
    Network
  },
  data() {
    return {
      websock: null,
      data: Object,
      host: Object,
      cpu: Object,
      memory: Object,
      disk: Object,
      network: Object,
    }
  },
  created() {
    this.initWebSocket();
  },
  unmounted() {
    this.websock.close() //离开路由之后断开websocket连接
  },
  methods: {
    initWebSocket() {
      const wsuri = "ws://10.168.1.83:4000/ws";
      this.websock = new WebSocket(wsuri);
      this.websock.onmessage = this.websocketonmessage;
      this.websock.onopen = this.websocketonopen;
      this.websock.onerror = this.websocketonerror;
      this.websock.onclose = this.websocketclose;
    },
    websocketonopen(){ //连接建立之后执行send方法发送数据
      let actions = {"test":"12345"};
      this.websocketsend(JSON.stringify(actions));
    },
    websocketonerror(){//连接建立失败重连
      this.initWebSocket();
    },
    websocketonmessage(e){ //数据接收
      this.data = JSON.parse(e.data);
      this.host = this.data.Host
      this.cpu = this.data.CPU
      this.memory = this.data.Memory
      this.disk = this.data.Disks
      this.network = this.data.NetDev
      //console.log(this.data)
    },
    websocketsend(Data){//数据发送
      this.websock.send(Data);
    },
    websocketclose(e){  //关闭
      console.log('断开连接',e);
    },
    notEmpty(obj) {
      if (Object.keys(obj).length > 0) {
        return true
      }
      return false
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  /* margin-top: 0px; */
}
</style>