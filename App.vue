<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png" />
  <HelloWorld msg="Hello Vue 3.0 + Vite" /> -->
  <header>天气查询</header>
  <div class="container flex">
    <div class="centerbox flex5">
      <echarts-five @clickChild="clickEven"></echarts-five>
    </div>
    <div class="leftbox flex3">
      <Item class="box"><testweather :result="result"> </testweather></Item>
      <Item class="box">
        <!-- <testweather :result="result"> </testweather> -->
        <juzi />
      </Item>
    </div>
    <!-- <div class="rightbox flex3">
      <Item class="box"> 待更新，暂时还没想到放什么 </Item>
      <Item class="box">待更新，暂时还没想到放什么</Item>
    </div> -->
  </div>
</template>
<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  /* border: 1px solid black; */
}
#app {
  margin-top: 0;
}

header {
  margin-bottom: 10px;
}
.flex {
  display: flex;
}
.flex3 {
  flex: 3;
}
.flex4 {
  flex: 4;
}
.flex5 {
  flex: 5;
}
.box {
  margin-left: 5%;
  margin-right: 5%;
}
.container {
  height: 100%;
}
.centerbox {
  height: 800px;
  border: 1px solid black;
}
</style>
<script setup>
import Item from "./components/Item.vue";
import juzi from "./pages/juzi.vue";
import echartsOne from "./components/echartsOne.vue";
import echartsFive from "./components/echartsFive.vue";
import testweather from "./pages/testweather.vue";
// import yiqing from "./pages/yiqing.vue";
import axios from "axios";
// import HelloWorld from "./components/HelloWorld.vue";
import { provide, ref } from "vue";
import * as echarts from "echarts";
// 设置axios的基准路径
axios.defaults.baseURL = "http://localhost:8888";
provide("echarts", echarts);
provide("axios", axios);

const result = ref("北京");
const clickEven = (val) => {
  // console.log("app里的", val);
  result.value = val;
  console.log("app里的result", result.value);
  // provide("result", result);
};
// provide("clickcity", clickcity);
</script>
