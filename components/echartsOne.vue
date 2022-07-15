<template>
  <div id="main"></div>
</template>

<script setup>
import { inject, onMounted, ref } from "vue";
import { theme } from "../assets/theme";
let $echarts = inject("echarts");
let $axios = inject("axios");
// 处理请求数据
let data = ref({});
let xdata = ref([]);
let ydata = ref([]);
function setDate() {
  xdata = data.map((v) => v.title);
  ydata = data.map((v) => v.num);
  // console.log(xdata, ydata);
}
async function getState() {
  data = await $axios({ url: "/one/data" });
  //  data=oneData.data.chartOne.chartDate
  data = data.data.chartOne.chartDate;
}
onMounted(() => {
  let myChart = $echarts.init(document.getElementById("main"), theme);
  getState().then(() => {
    setDate();
    myChart.setOption({
      grid: {
        top: "5%",
        left: "2%",
        right: "10%",
        bottom: "5%",
        containLabel: true,
      },
      xAxis: {
        type: "value",
      },
      yAxis: { type: "category", data: xdata, aligh: "right" },
      series: [{ data: ydata, type: "bar", label: { show: true } }],
    });
  });
});
// console.log($echarts, $axios);
</script>
<script></script>
<style>
#main {
  width: 100%;
  height: 100%;
  /* border: 1px solid black; */
}
</style>
