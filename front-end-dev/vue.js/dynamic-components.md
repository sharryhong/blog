---
description: 2022-07-11 월
---

# Dynamic Components

> 이슈: 마크업개발자분이 Button 컴포넌트 개발 중에 props href가 있으면 router-link로 element를 변경하고 싶으시다는 요청이 있었습니다. &#x20;

* 참고 링크 : [https://vuejs.org/guide/essentials/component-basics.html#dynamic-components](https://vuejs.org/guide/essentials/component-basics.html#dynamic-components)&#x20;

> Button.vue

```
<template>
  <component
    :is="componentType"
    :to="href"
    ...
    
  </component>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  props: {
    href: {
      type: String,
    },
    ...
  },
  computed: {
    componentType() {
      return this.href ? "router-link" : "button";
    },
  },
});
</script>
```
