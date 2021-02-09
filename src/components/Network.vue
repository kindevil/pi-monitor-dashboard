<!--
 * @Author: jia
 * @LastEditTime: 2021-02-09 13:18:54
 * @FilePath: /monitor/src/components/Network.vue
 * @Date: 2021-02-08 12:19:52
 * @Software: VS Code
-->
<template>
    <div class="card mt-5 mb-10">
        <div class="card-header">
            <div class="card-title h5">网络设备</div>
        </div>
        
        <div class="card-body">
            <div v-for="dev in data">
                <Netdev :data="dev" />
            </div>
        </div>
    </div>
</template>

<script>
import Netdev from './Netdev.vue'

export default {
    name: "Network",
    components: {
        Netdev,
    },
    props: {
        data: Object,
    },
    methods: {
        html() {
            for (var i=0; i < this.data.length; i++) {
                var box = document.getElementById('netdev');
                box.innerHTML += '<div id=\"network-'+this.data[i].Name+'\">';
                box.innerHTML += '<Netdev />';
                box.innerHTML += '</div>';
            }
        }
    },
}
</script>