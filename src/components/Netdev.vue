<!--
 * @Author: jia
 * @LastEditTime: 2021-02-09 11:51:00
 * @FilePath: /monitor/src/components/Netdev.vue
 * @Date: 2021-02-08 12:58:53
 * @Software: VS Code
-->
<template>
    <div class="mb-10">
        <div v-bind:id="dynamicId" class="chart"></div>
        <div class="columns bg-gray">
            <div class="column col-3 text-center rounded">
                <div class="text-tiny text-bold">流入</div>
                <div>{{ bytesRound(this.data.Recv)}}</div>
            </div>
            
            <div class="column col-3 text-center rounded">
                <div class="text-tiny text-bold">流出</div>
                <div>{{ bytesRound(this.data.Send)}}</div>
            </div>

            <div class="column col-3 text-center rounded">
                <div class="text-tiny text-bold">IP地址</div>
                <div v-for="dev in this.data.Addrs">
                    <div>{{ dev.addr }}</div>
                </div>
            </div>

            <div class="column col-3 text-center rounded">
                <div class="text-tiny text-bold">MAC地址</div>
                <div>{{ this.data.HardwareAddr}}</div>
            </div>
        </div>
    </div>
</template>

<script>
import * as echarts from 'echarts/core';
import {
    TitleComponent,
    TooltipComponent,
    GridComponent
} from 'echarts/components';
import {
    LineChart
} from 'echarts/charts';
import {
    CanvasRenderer
} from 'echarts/renderers';

echarts.use(
    [TitleComponent, TooltipComponent, GridComponent, LineChart, CanvasRenderer]
);

export default {
    name: "Netdev",
    props: {
        data: Object,
    },
    data() {
        return{
            chartDom: null,
            option: null,
            now: +new Date(),
            oneDay: 24 * 3600 * 1000,
            value: Math.random() * 1000,
            dynamicId: 'network-'+this.data.Name,
            in_data: new Array(),
            out_data: new Array(),
        }
    },
    methods: {
        chart() {
            this.chartDom = echarts.init(document.getElementById('network-'+this.data.Name));
 
            this.option = {
                title: {
                    text: '网卡 - ' + this.data.Name
                },
                tooltip: {
                    trigger: 'axis',
                    formatter: function (params) {
                        console.log(params)
                        //params = params[0];
                        //var date = new Date(params.name);
                        return params;
                    },
                    axisPointer: {
                        animation: true
                    }
                },
                xAxis: {
                    name: '时间',
                    type: 'time',
                    splitLine: {
                        show: true
                    },
                    maxInterval: 3600*1*1000,
                },
                yAxis: {
                    name: '流量',
                    type: 'value',
                    boundaryGap: [0, '100%'],
                    splitLine: {
                        show: true
                    },
                    axisLabel: {
                        formatter: function(value, index) {
                            if (value<0){
                                return 0+"B";
                            }else if (value<1024){
                                value=value/1;
                                return value.toFixed(1)+"B";
                            }else if (value<1048576){
                                value=value/1024;
                                return value.toFixed(1)+"KB";
                            }else if (value<1048576000){
                                value=value/1048576;
                                return value.toFixed(1)+"MB";
                            }else{
                                value=value/1048576000;
                                return value.toFixed(1)+"GB";
                            }
                        },
                    }
                },
                series: [{
                    name: '流入',
                    type: 'line',
                    showSymbol: false,
                    hoverAnimation: false,
                    smooth: true,
                    data: this.in_data
                },{
                    name: '流出',
                    type: 'line',
                    showSymbol: false,
                    hoverAnimation: false,
                    smooth: true,
                    data: this.out_data
                }]
            }

            this.chartDom.setOption(this.option);
        },
        bytesRound(number, round)
        {
            if (number<0){
                var last=0+"B/s";
            }else if (number<1024){
                number=number/1;
                var last=number.toFixed(1)+"B/s";
            }else if (number<1048576){
                number=number/1024;
                var last= number.toFixed(1)+"KB/s";
            }else if (number<1048576000){
                number=number/1048576;
                var last=number.toFixed(1)+"MB/s";
            }else{
                number=number/1048576000;
                var last=number.toFixed(1)+"GB/s";
            }
            return last;
        }
    },
    mounted() {
        this.chart();
    },
    watch: {
        data: function() {
            var currtime = new Date();
            var format = currtime.getFullYear()+"-"+currtime.getMonth()+"-"+currtime.getDay()+" "+ currtime.getHours() + ":" + currtime.getMinutes() + ":" + currtime.getSeconds()

            this.in_data.push([
                format,this.data.Recv
            ])

            this.out_data.push([
                format,this.data.Send
            ])

            if (this.in_data.length >= 600) {
                this.in_data.shift()
                this.out_data.shift()
            }

            this.chartDom.setOption(this.option);
        }
    },
}
</script>