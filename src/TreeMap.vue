<template>
    <div id="tree-map"></div>
</template>
<script>
import echarts from "echarts";
export default {
    name: "TreeMap",
    data() {
        return {
            data: "",
            myChart: ""
        };
    },
    mounted() {
        fetch("http://localhost:3000/api")
            .then(res => res.json())
            .then(data => (this.data = data.data));
        this.myChart = echarts.init(document.getElementById("tree-map"));
    },
    computed: {
        option() {
            return {
                tooltip: {
                    trigger: "item",
                    triggerOn: "mousemove"
                },
                series: [
                    {
                        type: "treemap",
                        data: this.data.children,
                        visibleMin: 2,
                        label: {
                            show: true,
                            formatter: "{b}"
                        },
                        levels: [
                            {
                                itemStyle: {
                                    normal: {
                                        borderWidth: 0,
                                        gapWidth: 5
                                    }
                                }
                            },
                            {
                                itemStyle: {
                                    normal: {
                                        gapWidth: 1
                                    }
                                }
                            },
                            {
                                colorSaturation: [0.35, 0.5],
                                itemStyle: {
                                    normal: {
                                        gapWidth: 1,
                                        borderColorSaturation: 0.6
                                    }
                                }
                            }
                        ]
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
#tree-map {
    width: 800px;
    height: 800px;
    border: 1px solid #00a6f3;
}
</style>
