---
description: 2022-07-29 금
---

# Vue Image Skeleton

### 이슈

기존 코드에 `<img lazy-src=”ico_lazy.png”` 와 같은 코드가 있었는데 html img 태그의 기본 속성엔 lazy-src가 없습니다.&#x20;

개발자의 의도는 이미지가 로드 되기 전에 보여질 이미지를 지정하고 싶었던 것 같아서 검색해보니 vuetify v-img 컴포넌트에 있는걸 그대로 넣은 것 같았습니다. [https://vuetifyjs.com/en/components/images/](https://vuetifyjs.com/en/components/images/)&#x20;



Img 컴포넌트를 개발하면서 스켈레톤에 대해 좀 더 자세히 알아보는 계기가 되었습니다.

마크업개발자와 함께 코드리뷰를 진행하며 리팩토링 하고 있는데, 서로 좋은 코드를 향한 방법을 제시해주고 있어 만족스럽네요!&#x20;



#### 자료

[https://learnvue.co/tutorials/vue-skeleton-loading](https://learnvue.co/tutorials/vue-skeleton-loading) // Vue3 Suspense를 응용&#x20;

\=> 현재 개발중인 프로젝트는 Vue2로 되어있고, 복잡하지 않은 UI라 최대한 간단하게 Img 컴포넌트를 만들어서 적용하기로 하였습니다.&#x20;

#### 참고 자료&#x20;

[https://renatello.com/vue-js-image-loaded/](https://renatello.com/vue-js-image-loaded/)&#x20;

### 해결 코드&#x20;

> Img.vue&#x20;

* Vue img @load 이벤트로 이미지가 완전히 로드된 시점 후에 src 경로를, 그 전엔 default image를 적용시킵니다.

```
<template>
  <img
    :src="
      isLoaded && src ? src : `${require('@/assets/images/' + defaultImage)}`
    "
    :alt="alt"
    @load="onLoadImage"
  />
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  props: {
    src: {
      type: String,
    },
    defaultImage: {
      type: String,
      default: "ico_default.png",
    },
    alt: {
      type: String,
      default: "",
    },
  },
  data() {
    return {
      isLoaded: false,
    };
  },
  methods: {
    onLoadImage() {
      this.isLoaded = true;
    },
  },
});
</script>

<style lang="scss" scoped>
img {
  width: inherit;
  height: inherit;
  object-fit: cover;
}
</style>
```

> 사용 예&#x20;

```
<Img :src="srcUrl" defaultImage="ico_profile.png" />
```

