---
description: '2022-08-04'
---

# Pagination Component

### 이슈&#x20;

* 기존 사용중이던 Pagination 컴포넌트 내부 코드의 복잡함
* 스크립트가 여러 파일에 흩어져 있어서 유지보수의 어려움

#### 참고 자료&#x20;

* [https://19-97.tistory.com/130](https://19-97.tistory.com/130)&#x20;
* startPage, endPage 구하는 식만 참고하였고, pagingList의 경우에는 Array.from 메소드로 리팩토링하였습니다.

### 해결 코드&#x20;

> Pagination.vue&#x20;

```
<template>
  <div>
    <ol>
      <li>
        <Button
          :disabled="pageOptions.first"
          @click="onClick(1)"
        >
          처음 페이지
        </Button>
      </li>
      <li>
        <Button
          :disabled="pageOptions.first"
          @click="onClick(pageOptions.page - 1)"
        >
          이전 페이지
        </Button>
      </li>
      <li v-for="page in pagingList" :key="page">
        <button
          type="button"
          :class="['btn_page_number', { selected: pageOptions.page === page }]"
          @click="onClick(page)"
        >
          {{ page }}
        </button>
      </li>
      <li>
        <Button
          :disabled="pageOptions.last"
          @click="onClick(pageOptions.page + 1)"
        >
          다음 페이지
        </Button>
      </li>
      <li>
        <Button
          :disabled="pageOptions.last"
          @click="onClick(pageOptions.totalPages)"
        >
          마지막 페이지
        </Button>
      </li>
    </ol>
  </div>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  props: {
    pageOptions: {
      type: Object,
    },
    // 화면에 출력할 페이지 갯수
    totalVisible: {
      type: Number,
      default: 10,
    },
  },
  computed: {
    // 페이지 시작 번호
    startPage() {
      return (
        Math.trunc((this.pageOptions.page - 1) / this.totalVisible) *
          this.totalVisible +
        1
      );
    },
    // 페이지 끝 번호
    endPage() {
      let end = this.startPage + this.totalVisible - 1;
      return end < this.pageOptions.totalPages
        ? end
        : this.pageOptions.totalPages;
    },
    // 화면에 표시할 페이지 리스트 
    pagingList() {
      return this.rangeNumberToArray(this.startPage, this.endPage);
    },
  },
  methods: {
    onClick(page: number) {
      this.$emit("click", page);
    },
    // start, end 숫자로 배열 만드는 기능 
    rangeNumberToArray(start: number, end: number) {
      return Array.from({ length: end - start + 1 }, (_, i) => start + i);
    },
  },
});
</script>

<style scoped lang="scss">
...
</style>

```

> 사용 예&#x20;

```
<template>
  ...
  <Pagination :pageOptions="pageOptions" @click="onClickPage" /> 
</template>

<script lang="ts">
  export default Vue.extend({
    data: () => ({
      pageOptions: {
      page: 1,
      size: 10,
      totalElements: 0,
      totalPages: 0,
      first: false,
      last: false,
    },
    ...

    methods: {
      onClickPage(page: number) {
        this.pageOptions.page = page;
        this.fetchNotice();
      },
            
```
