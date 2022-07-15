<template>
  <div id="weatherShow"></div>
  <!-- {{ props.result }} -->
</template>

<script setup>
import { inject, onMounted, ref } from "vue";
import axios from "axios";
const instance = axios.create({
  baseURL: "https://restapi.amap.com/v3",
  headers: { key: "095343110176dde7105ce8af739d09bf" },
});
const $echart = inject("echarts");
// 接收地图点击的地点数据
const props = defineProps(["result"]);
let lists = ref([]);
let daytemp = ref([]);
let nighttemp = ref([]);
let date = ref([]);
let citydata = ref("");
// 获取天气数据
async function getData() {
  const data = await instance.get("/weather/weatherInfo", {
    params: {
      city: props.result,
      extensions: "all",
    },
  });
  lists = data.data.forecasts;
  //   console.log("表3原始所有数据：", lists);
}
// 设置数据
const setData = () => {
  daytemp = lists[0].casts.map((v) => v.daytemp);
  nighttemp = lists[0].casts.map((v) => v.nighttemp);
  date = lists[0].casts.map((v) => v.date);
  citydata = lists[0].city;
  console.log(
    "3数据：",
    citydata,
    "白天气温：",
    daytemp,
    "夜晚气温：",
    nighttemp,
    date
  );
};
onMounted(() => {
  getData().then(() => {
    setData();
  });
  console.log("表3 挂载成功", props.result);
  let echartThr = $echart.init(document.getElementById("weatherShow"));
  echartThr.setOption({
    title: {
      text: "地图上省份的天气",
    },
    xAxis: {
      type: "category",
      boundaryGap: false,
      data: date,
    },
    yAxis: {
      type: "value",
      axisLabel: {
        formatter: "{value} °C",
      },
    },
    // series: [
    //   {
    //     name: "白天",
    //     type: "line",
    //     data: daytemp,
    //   },
    // ],
  });
});
</script>

<style scoped>
#weatherShow {
  width: 100%;
  height: 100%;
}
</style>
