---
description: '2022-05-13'
---

# composition-api 사용하여 menu 의 outside click 시 close

### 이슈

dropdown 처럼 클릭시 열고 닫히는 컴포넌트의 **바깥 부분을 클릭**하면 닫히게 하는 기능. \
여러 곳에 재사용이 가능하기 때문에 composition-api를 사용하여 React hooks처럼 만들어보았습니다.



### useClickOutside.ts&#x20;

* Vue2를 사용중이기 때문에 @vue/composition-api 를 설치 후 사용한다.&#x20;
* Vue3를 사용한다면 \`_import_ { ref, onBeforeUnmount, onMounted } _from_ "vue";\` 로 수정하면 된다.&#x20;

```
import { ref, onBeforeUnmount, onMounted } from "@vue/composition-api";

// 사용하는 곳에서 element의 class명을 argument로 보낸다. 
export const useClickOutside = (el: string) => {
  const isClickOutside = ref<boolean>(false);
  const close = (e: Event) => {
    if (
      e.target instanceof HTMLElement &&
      document.querySelector(el).contains(e.target)
    ) {
      isClickOutside.value = false;
      return;
    }
    isClickOutside.value = true;
  };
  onBeforeUnmount(() => {
    document.removeEventListener("click", close);
  });
  onMounted(() => {
    document.addEventListener("click", close);
  });
  return {
    isClickOutside,
  };
};

```

> 사용 예&#x20;

```
<script lang="ts">
import Vue from "vue";
import { useClickOutside } from "@/hooks/useClickOutside";

export default Vue.extend({
  setup() {
    const { isClickOutside } = useClickOutside('.classname');
    return { isClickOutside };
  },
  watch: {
    isClickOutside() {
      if (this.isClickOutside) {
        // 바깥 부분 클릭시 처리할 코드 
        this.isShow = false;
      }
    },
  },
```

