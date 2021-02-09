<!--
 * @Author: jia
 * @LastEditTime: 2021-02-08 13:35:49
 * @FilePath: /monitor/src/components/Info.vue
 * @Date: 2021-02-08 11:54:01
 * @Software: VS Code
-->
<template>
    <div class="card mt-5">
        <div class="card-header">
            <div class="card-title h5">设备信息</div>
            <div class="card-subtitle text-gray">
            IP地址: 192.168.1.32
            </div>
        </div>
        <div class="card-body">
            <dl>
            <dt>主机名</dt>
            <dd>{{ data.Hostname}}</dd>

            <dt>操作系统</dt>
            <dd>{{ data.OS }}</dd>

            <dt>启动时间</dt>
            <dd>{{ data.BootTime}}</dd>

            <dt>厂商</dt>
            <dd>{{ data.Vendor }}</dd>

            <dt>硬件版本</dt>
            <dd>{{ data.Model }}</dd>

            <dt>序列号</dt>
            <dd>{{ data.Serial }}</dd>

            <dt>内核信息</dt>
            <dd>{{ data.Kernal }}</dd>
            </dl>
        </div>
    </div>
</template>

<script>
export default {
    name: "Info",
    props: {
        data: Object,
    },
    watch: {
        data: function(val) {
            //console.log(val)
        }
    }
}
</script>