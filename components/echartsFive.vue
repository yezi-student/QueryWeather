<template>
  <div id="mapChart"></div>
</template>

<script setup>
import { inject, onMounted } from "vue";
import { chinamapChart } from "../assets/mapData";
let $echarts = inject("echarts");
const emit = defineEmits(["clickChild"]);
onMounted(() => {
  let mapChart = $echarts.init(document.getElementById("mapChart"));
  // 注册当前使用的地图名
  $echarts.registerMap("chianMap", chinamapChart);
  mapChart.setOption({
    geo: {
      type: "map",
      map: "chianMap",
      roam: true,
      zoom: 1.5,
      center: [116.404165, 39.910966],
      label: {
        show: true,
        color: "#000000",
        fontSize: 10,
      },
      itemStyle: {
        areaColor: "#4ea397",
        borderColor: "#22c3aa",
        shadowColor: "rgba(230,130,70,0.5)",
        shadowBlur: 30,
        emphasis: {
          focus: "self",
        },
      },
    },
    // 散点图
    tooltip: {
      trigger: "item",
    },
    // visualMap: {
    //   type: "continumous",
    //   min: 100,
    //   max: 5000,
    //   calculable: true,
    //   inRange: {
    //     color: ["#59c4e6", "#516b91", "#eab8da", "#93b7e3"],
    //   },
    //   textStyle: {
    //     color: "#fff",
    //   },
    // },
    series: [
      {
        type: "scatter",
        itemStyle: {
          color: "#fba748",
        },
        data: [
          {
            name: "北京市",
            value: [116.46, 39.92],
          },
        ],
        coordinateSystem: "geo",
      },
    ],
  });
  mapChart.on("click", function clickChild(params) {
    emit("clickChild", params.name);
  });
});
</script>

<style>
#mapChart {
  width: 100%;
  height: 100%;
}
</style>
