---
description: '2021-09-20'
---

# element 내부 scroll 될 때, ScrollTop 값에 따라 div를 보이게, 안보이게 제어하기

실제 코드에는  좀 더 조건이 있지만,  간단하게 정리해보았습니다. 

```text
<template>
// 스크롤될 element
<div ref="scrollEl"> 
  ...
  // 스크롤 아래로 내리면 사라질 element
  <div class="mask-scroll" v-if="isMaskScroll"> 
    ...
  </div>
</div>
```

```text
<script>
import _throttle from 'lodash/throttle';

export default{
  data() {
    return {
      scrollElScrollTop: 0,
    }
  },
  computed: {
    isMaskScroll() {
      if(this.scrollElScrollTop > 50) return false;
      else return true;
    },
  },  
  mounted() {
    this.$nextTick(() => {
      this.$refs.scrollEl.addEventListener('scroll', this.handleScroll);
    });
  },
  beforeDestroy() {
    this.$refs.scrollEl.removeEventListener('scroll', this.handleScroll);
  },
  methods: {
    handleScroll: _throttle(function() {
      this.scrollElScrollTop = this.$refs.scrollEl.scrollTop;
    }, 100),
```



