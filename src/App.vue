<template>
    <div class="container">
        <div class="title">知识结构图</div>
        <Tree></Tree>
        <div class="title">从考查次数分析</div>
        <TreeMap></TreeMap>
        <div class="title">从时间线和学生掌握程度分析</div>
        <TimeLine></TimeLine>
    </div>
</template>
<script>
import echarts from "echarts";
import Tree from "./Tree";
import TreeMap from "./TreeMap";
import TimeLine from "./TimeLine";
export default {
    name: "App",
    components: {
        Tree,
        TreeMap,
        TimeLine
    }
};
</script>
<style scoped>
.container {
    display: flex;
    flex-direction: column;
    align-items: center;
}
.title {
    margin-top: 15px;
    margin-bottom: 15px;
}
</style>
