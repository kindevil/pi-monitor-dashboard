<!--
 * @Author: jia
 * @LastEditTime: 2021-02-09 14:13:11
 * @FilePath: /monitor/src/components/Memory.vue
 * @Date: 2021-02-08 11:18:27
 * @Software: VS Code
-->
<template>
    <div class="card mt-5">
        <div class="card-header">
            <div class="card-title h5">内存</div>
        </div>

        <div class="card-body">
            <div id="memory" class="chart"></div>
        </div>

        <div class="card-footer">
            <div class="columns">
                <div class="column col-4">
                    <div class="text-center bg-blue rounded">
                        <div class="text-bold">总共</div>
                        <div v-if="noEmpty()">{{ data.Total }}MB</div>
                    </div>
                </div>
                <div class="column col-4">
                    <div class="text-center bg-blue rounded">
                        <div class="text-bold">已使用</div>
                        <div v-if="noEmpty()">{{ data.Used }}MB</div>
                    </div>
                </div>
                <div class="column col-4">
                    <div class="text-center bg-blue rounded">
                        <div class="text-bold">未使用</div>
                        <div v-if="noEmpty()">{{ data.Free }}MB</div>
                    </div>
                </div>
            </div>

            <div class="columns bg-gray text-center mt-10 rounded">
                <div class="column col-3">
                    <div class="text-bold">SHARED</div>
                    <div v-if="noEmpty()">{{ data.Shared}}</div>
                </div>
                <div class="column col-3">
                    <div class="text-bold">CACHE</div>
                    <div v-if="noEmpty()">{{ data.Cached }}</div>
                </div>
                <div class="column col-3">
                    <div class="text-bold">AVAILABLE</div>
                    <div v-if="noEmpty()">{{ data.Available }}</div>
                </div>
                <div class="column col-3">
                    <div class="text-bold">SWAP</div>
                    <div v-if="noEmpty()">{{ data.SwapTotal - data.SwapFree }} / {{ data.SwapTotal }}</div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import * as echarts from 'echarts/core';
import {
    GaugeChart
} from 'echarts/charts';
import {
    CanvasRenderer
} from 'echarts/renderers';

echarts.use(
    [GaugeChart, CanvasRenderer]
);

export default {
    name: "Memory",
    props: {
        data: Object,
    },
    data() {
        return{
            chartDom: null,
            option: null,
        }
    },
    methods: {
        noEmpty() {
            if (Object.keys(this.data).length != 0) {
                return true
            }
            return false
        },
        chart() {
            this.chartDom = echarts.init(document.getElementById('memory'));
            this.option = {
                series: [{
                    type: 'gauge',
                    startAngle: 180,
                    endAngle: 0,
                    min: 0,
                    max: 100,
                    splitNumber: 10,
                    itemStyle: {
                        color: '#2463EB',
                    },
                    progress: {
                        show: true,
                        roundCap: true,
                        width: 12
                    },
                    pointer: {
                        icon: 'path://M2090.36389,615.30999 L2090.36389,615.30999 C2091.48372,615.30999 2092.40383,616.194028 2092.44859,617.312956 L2096.90698,728.755929 C2097.05155,732.369577 2094.2393,735.416212 2090.62566,735.56078 C2090.53845,735.564269 2090.45117,735.566014 2090.36389,735.566014 L2090.36389,735.566014 C2086.74736,735.566014 2083.81557,732.63423 2083.81557,729.017692 C2083.81557,728.930412 2083.81732,728.84314 2083.82081,728.755929 L2088.2792,617.312956 C2088.32396,616.194028 2089.24407,615.30999 2090.36389,615.30999 Z',
                        length: '62%',
                        width: 16,
                        offsetCenter: [0, '5%']
                    },
                    axisLine: {
                        roundCap: true,
                        lineStyle: {
                            width: 12
                        }
                    },
                    axisTick: {
                        splitNumber: 2,
                        lineStyle: {
                            width: 2,
                            color: '#999'
                        }
                    },
                    splitLine: {
                        length: 12,
                        lineStyle: {
                            width: 3,
                            color: '#999'
                        }
                    },
                    axisLabel: {
                        distance: 16,
                        color: '#999',
                        fontSize: 14
                    },
                    title: {
                        show: false
                    },
                    detail: {
                        backgroundColor: '#fff',
                        borderColor: '#999',
                        borderWidth: 2,
                        width: '70%',
                        lineHeight: 35,
                        height: 35,
                        borderRadius: 8,
                        offsetCenter: [0, '65%'],
                        valueAnimation: true,
                        formatter: function (value) {
                            return '{value|' + value.toFixed(1) + '}{unit|%}';
                        },
                        rich: {
                            value: {
                                fontSize: 32,
                                fontWeight: 'bolder',
                                color: '#777'
                            },
                            unit: {
                                fontSize: 16,
                                color: '#999',
                                padding: [0, 0, -10, 10]
                            }
                        }
                    },
                    data: [{
                        value: 0
                    }]
                }]
            };
            
            this.chartDom.setOption(this.option);
        },
    },
    mounted() {
        this.chart()
    },
    watch: {
        data: function() {
            this.option.series[0].data[0].value = this.data.UsedPercent
            this.chartDom.setOption(this.option)
        }
    }
}
</script>