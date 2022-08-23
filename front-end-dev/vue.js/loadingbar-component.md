---
description: 2022-07-05, 2022-08-22
---

# LoadingBar Component

### 이슈 1.

개발 중인 프로젝트에 테이블 형식의 UI가 많은데, Loading spinner는 어울리지 않아 bar 형태로 추가하기로 하였습니다.&#x20;

* 참고 디자인 : [https://vuetifyjs.com/en/components/data-tables/#loading](https://vuetifyjs.com/en/components/data-tables/#loading)&#x20;

### LoadingBar Component 1. &#x20;

* Reflow, Repainting이 발생하지 않도록 transform 속성을 이용하여 애니메이션을 처리하였습니다.

> LoadingBar.vue

```typescript
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



### 이슈 2.&#x20;

LoadingBar 컴포넌트가 table thead 내로 들어가게 되는데, table내에 div요소가 들어가면 웹표준에 맞지 않는다는 것을 알게되어 마크업 개발자분과 함께 수정하였습니다.

### LoadingBar Component 2.&#x20;

> LoadingBar.vue&#x20;

```typescript
<template>
  // loading시 table thead 내에 tr로 들어가게됩니다. 
  <tr class="loading-area" :style="styles">
    <th :colspan="colspan">
      <span class="loading-bar"></span>
    </th>
  </tr>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  props: {
    styles: {
      type: String,
    },
  },
  data: () => ({
    colspan: 0,
  }),
  mounted() {
    this.$nextTick(() => {
      // parent table cell갯수를 가져와서 calspan 값으로 할당합니다. 
      this.colspan = this.$el
        .closest("table")
        .querySelector("tr:first-child").cells.length;
    });
  },
});
</script>

<style lang="scss" scoped>
.loading-area {
  position: absolute;
  z-index: 10;
  width: 100%;
  height: 4px;
  background-color: rgba(4, 122, 255, 0.25);
  overflow: hidden;
  > th {
    height: auto;
  }
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
