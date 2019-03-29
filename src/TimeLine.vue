<template>
    <div id="time-line"></div>
</template>
<script>
import echarts from "echarts";
import dayjs from "dayjs";
export default {
    name: "TimeLine",
    data() {
        return {
            data: [],
            myChart: "",
            links: []
        };
    },
    mounted() {
        fetch("http://localhost:3000/api")
            .then(res => res.json())
            .then(data => {
                console.log(data.data);
                const values = data.data.children.map((v, i) => [
                    dayjs()
                        .add(i, "day")
                        .format("YYYY/M/D"),
                    v.value
                ]);
                this.data = values;
                this.links = values
                    .map((v, i) => ({ source: i, target: i + 1 }))
                    .slice(0, -1);
            });
        this.myChart = echarts.init(document.getElementById("time-line"));
    },
    computed: {
        option() {
            return {
                xAxis: {
                    type: "time",
                    min: v => v.min - 6 * 60 * 60 * 1000,
                    max: v => v.max + 6 * 60 * 60 * 1000
                },
                yAxis: {
                    type: "value"
                },
                series: [
                    {
                        type: "graph",
                        layout: "none",
                        coordinateSystem: "cartesian2d",
                        symbolSize: 40,
                        label: {
                            normal: {
                                show: true
                            }
                        },
                        edgeSymbol: ["circle", "arrow"],
                        edgeSymbolSize: [4, 10],
                        data: this.data,
                        links: this.links,
                        lineStyle: {
                            normal: {
                                color: new echarts.graphic.LinearGradient(
                                    0,
                                    0,
                                    0,
                                    1,
                                    [
                                        { offset: 0, color: "#04B4F0" },
                                        { offset: 1, color: "#710F42" }
                                    ]
                                ),
                                width: 3
                                // color: "#2f4554"
                            }
                        }
                    }
                ]
            };
        }
    },
    watch: {
        option() {
            this.data && this.myChart.setOption(this.option);
        }
    }
};
</script>
<style scoped>
#time-line {
    width: 800px;
    height: 800px;
    border: 1px solid #00a6f3;
}
</style>
