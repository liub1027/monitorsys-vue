<template>
    <div :id="id"  :style="style"></div>
</template>

<script>
    export default {
        name: "ShowLocalMem",
        data(){
            return{
                chart: '',
                option:{
                    title: {
                        subtext: '本机内存使用率',
                        left:'left',
                        subtextStyle:{
                            color:'#dd7b33'
                        }
                    },
                    tooltip: {
                        trigger: 'axis',
                        formatter: ' {b}<br/>{a} : {c} %'
                    },
                    legend: {
                        data: ['本机内存使用率']
                    },
                    grid: {
                        left: '3%',
                        right: '4%',
                        bottom: '3%',
                        containLabel: true
                    },
                    toolbox: {
                        feature: {
                            saveAsImage: {}
                        }
                    },
                    xAxis: {
                        type: 'category',
                        boundaryGap: true,
                        data: []
                    },
                    yAxis: {
                        type: 'value'
                    },
                    series: [
                        {
                            name: '本机内存使用率',
                            type: 'line',
                            stack: '总量',
                            data: []
                        }
                    ]
                },

            }
        },
        props: {
            id: {
                type: String
            },
            width: {
                type: String,
                default: "100%"
            },
            height: {
                type: String,
                default: "260px"
            },
            data: Array,
        },
        computed: {
            style() {
                return {
                    height: this.height,
                    width: this.width
                }
            }
        },
        mounted() {
            // 使用假数据替代后端API调用
            this.initMockData();
        },
        methods:{
            // 模拟内存使用率数据
            initMockData(){
                // 生成过去12小时的时间点和随机内存使用率
                let times = [];
                let memRates = [];
                
                for (let i = 11; i >= 0; i--) {
                    let now = new Date();
                    now.setHours(now.getHours() - i);
                    times.push(`${now.getHours()}:00`);
                    memRates.push(Math.floor(Math.random() * 40) + 40); // 40-80%之间的随机使用率
                }
                
                this.chart = this.$echarts.init(document.getElementById(this.id));
                this.option.xAxis.data = times;
                this.option.series[0].data = memRates;
                this.chart.setOption(this.option, true);
                window.addEventListener("resize", this.chart.resize);
            }
        }
    }
</script>

<style scoped>

</style>
