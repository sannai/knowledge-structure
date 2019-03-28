<template>
    <div id="tree"></div>
</template>
<script>
import echarts from "echarts";
export default {
    name: "Tree",
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
        this.myChart = echarts.init(document.getElementById("tree"));
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
                        type: "tree",
                        data: [this.data] || [],
                        top: "1%",
                        left: "6%",
                        bottom: "3%",
                        right: "6%",
                        symbolSize: 16,
                        // symbolSize: value => (value ? value * 3 : 16),
                        label: {
                            normal: {
                                position: "bottom",
                                fontSize: 14
                            }
                        },
                        leaves: {
                            label: {
                                normal: {
                                    position: "bottom"
                                }
                            }
                        },

                        expandAndCollapse: true,
                        animationDuration: 550,
                        animationDurationUpdate: 750
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
#tree {
    width: 800px;
    height: 800px;
    border: 1px solid #00a6f3;
}
</style>
