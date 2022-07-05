---
description: '2022-07-05'
---

# LoadingBar Component

> 개발 중인 프로젝트에 테이블 형식의 UI가 많은데, API 호출 로딩 spinner는 어울리지 않아 bar 형태로 추가하기로 하였습니다.&#x20;

* 참고 디자인 : [https://vuetifyjs.com/en/components/data-tables/#loading](https://vuetifyjs.com/en/components/data-tables/#loading)&#x20;



> LoadingBar.vue

```
<template>
  <div class="loading-area" :style="styles">
    <span class="loading-bar"></span>
  </div>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  props: {
    styles: {
      type: String,
    },
  },
});
</script>

<style scoped>
.loading-area {
  position: absolute;
  z-index: 10;
  width: 100%;
  height: 4px;
  background-color: rgba(4, 122, 255, 0.25);
  overflow: hidden;
}
.loading-bar {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: #047aff;
  animation: loading-bar 1s linear infinite;
}

@keyframes loading-bar {
  0% {
    transform: translateX(-100%);
    opacity: 0;
  }
  80% {
    transform: translateX(90%);
    opacity: 1;
  }
  100% {
    transform: translateX(100%);
    opacity: 0;
  }
}
</style>

```

> 사용 예

```
<LoadingBar v-if="isLoading" styles="width:1200px" />
```
