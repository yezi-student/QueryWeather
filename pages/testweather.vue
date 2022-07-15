<template>
  <div class="box">
    <div class="box1">白天-夜晚-未来三天天气对比图</div>
    <div class="box2">
      <input ref="city" :value="result" />
      <button @click="handleWeather">搜索</button>
    </div>

    <div id="weatherChart"></div>
    <div id="conclusion"></div>
  </div>
</template>

<script setup>
import axios from "axios";
import { inject, onMounted, ref } from "vue";
const props = defineProps(["result"]);
const $echarts = inject("echarts");
const instance = axios.create({
  baseURL: "https://restapi.amap.com/v3",
  headers: { key: "095343110176dde7105ce8af739d09bf" },
});
let city = ref("");
let lists = ref([]);
let daytemp = ref([]);
let nighttemp = ref([]);
let date = ref([]);
let dayweather = ref([]);
let citydata = ref("");
// 获取数据
async function getData() {
  const data = await instance.get("/weather/weatherInfo", {
    params: {
      city: city.value.value,
      extensions: "all",
    },
  });
  // console.log("原始所有数据：", data.data);
  lists = data.data.forecasts;
  console.log("2预报天气的数组：", lists);
}
// 设置数据
const setData = () => {
  daytemp = lists[0].casts.map((v) => v.daytemp);
  nighttemp = lists[0].casts.map((v) => v.nighttemp);
  date = lists[0].casts.map((v) => v.date);
  citydata = lists[0].city;
  dayweather = lists[0].casts.map((v) => v.dayweather);
  // console.log(
  //   "3数据：",
  //   citydata,
  //   "白天气温：",
  //   daytemp,
  //   "夜晚气温：",
  //   nighttemp,
  //   date
  // );
  console.log(dayweather);
};
// 鼠标事件
const handleWeather = () => {
  getData().then(() => {
    setData();
    const conclusion = document.getElementById("conclusion");
    conclusion.innerHTML = citydata + "晴雨表（今天及未来三天）：" + dayweather;
    let weatherChart = $echarts.init(document.getElementById("weatherChart"));
    weatherChart.setOption({
      grid: {
        top: "10%",
        left: "5%",
        right: "10%",
        bottom: "5%",
        containLabel: true,
      },
      //   title: {
      //     text: "未来三天白天-夜晚气温对比图",
      //   },
      tooltip: {
        trigger: "axis",
      },
      legend: {},
      toolbox: {
        show: true,
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
      series: [
        {
          name: "白天",
          type: "line",
          data: daytemp,
          //   markPoint: {
          //     data: [
          //       { type: "max", name: "Max" },
          //       { type: "min", name: "Min" },
          //     ],
          //   },
          markLine: {
            data: [{ type: "average", name: "Avg" }],
          },
        },
        {
          name: "夜晚",
          type: "line",
          data: nighttemp,
          //   markPoint: {
          //     data: [{ name: "周最低", value: -2, xAxis: 1, yAxis: -1.5 }],
          //   },
          markLine: {
            data: [
              { type: "average", name: "Avg" },
              [
                {
                  symbol: "none",
                  x: "90%",
                  yAxis: "max",
                },
                {
                  symbol: "circle",
                  label: {
                    position: "start",
                    formatter: "Max",
                  },
                  type: "max",
                  name: "最高点",
                },
              ],
            ],
          },
        },
      ],
    });
  });
};
onMounted(() => {
  console.log("1挂载成功");
  handleWeather();
});
</script>

<style scoped>
* {
  margin: 0;
  padding: 5px;
}
#weatherChart {
  width: 100%;
  height: 350px;
  /* border: 1px solid black; */
}
.box1,
.box2 {
  margin-bottom: 10px;
}
</style>
