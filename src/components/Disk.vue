<!--
 * @Author: jia
 * @LastEditTime: 2021-02-08 14:30:19
 * @FilePath: /monitor/src/components/Disk.vue
 * @Date: 2021-02-08 12:05:34
 * @Software: VS Code
-->
<template>
    <div class="card mt-5 mb-10">
        <div class="card-header">
            <div class="card-title h5">设备分区</div>
        </div>
        
        <div class="card-body">
            <table class="table">
                <thead>
                    <tr>
                    <th>设备</th>
                    <th>挂载点</th>
                    <th>容量</th>
                    </tr>
                </thead>
                <tbody v-for="disk in data">      
                    <tr>
                        <td>{{ disk.Name }}</td>
                        <td>{{ disk.Mountpoint }}</td>
                        <td>
                            <div class="columns">
                                <div class="column col-8">
                                    <div class="bar">
                                        <div class="bar-item " :style="{width: disk.UsedPercent+'%'}">{{ toFixed(disk.UsedPercent) }}%</div>
                                    </div>
                                </div>
                                <div class="column col-4">
                                    <span class="label label-success">{{ disk.Used }} / {{ disk.Total }}</span>
                                </div>
                            </div>
                        </td>
                    </tr>

                </tbody>
            </table>

        </div>
    </div>
</template>

<script>
export default {
    name: "Disk",
    props: {
        data: Object,
    },
    methods: {
        noEmpty() {
            if (Object.keys(this.data).length != 0) {
                return true
            }
            return false
        },
        toFixed(data) {
            return data.toFixed(1)
        }
    },
    watch: {

    },
}
</script>