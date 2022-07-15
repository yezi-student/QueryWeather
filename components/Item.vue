<template>
  <div class="sub">
    <div class="cover">
      <slot></slot>
    </div>
  </div>
</template>

<script setup></script>

<style scoped>
.sub {
  flex: 0 0 32%;
  position: relative;
  cursor: pointer;
  /* margin-right: 2%; */
  margin-bottom: 4%;
}
.cover {
  position: relative;
  /* height: 400px; */
  background: #fbfbfb !important;
  border-radius: 5px;
  /* font-size: 24px;
  text-align: center;
  line-height: 110px; */
  color: #000;
  transition: all 0.2s ease-in-out;
  font-weight: 200;
  border: 1px solid rgb(232 232 232/10%);
}
.cover:hover {
  box-shadow: 0 18px 32px -18px #000 !important;
  transform: translateY(-3px);
}
</style>
