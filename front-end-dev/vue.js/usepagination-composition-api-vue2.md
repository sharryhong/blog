---
description: '2022-08-19'
---

# usePagination - composition api (vue2)

### 이슈 1. 반복 코드 리팩토링&#x20;

Pagination 컴포넌트를 사용하는 곳에서 공통으로 반복해서 사용하는 코드를 composition api 를 사용하여 개선해보았습니다.

#### 기존 공통 코드&#x20;

```typescript
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
  }),
  ...
  methods: {
    async getNotice() {
      try {
        const response = await notice_api.getNotice(config);
        if (response.status === 200) {
          const data = response.data.items;
          this.pageOptions = {
            page: this.pageOptions.page,
            size: data.size,
            totalElements: data.totalElements,
            totalPages: data.totalPages,
            first: data.first,
            last: data.last,
          };

```

### 개선 코드&#x20;

> hooks/usePagination.ts

공통으로 사용할 data와 function을 관리&#x20;

```typescript
import { reactive } from "@vue/composition-api";

export interface PageType {
  page: number;
  size: number;
  totalElements?: number;
  totalPages: number;
  first: boolean;
  last: boolean;
}

interface ArgType {
  size: number;
}
interface ReturnType {
  pageOptions: PageType;
  setPageOptions: (page: number, response: PageType) => void;
}

// 한번에 보여줄 리스트 갯수(size)를 argument로 전달하고, 기본값은 10. optional로 생략가능 
export const usePagination = (arg?: ArgType): ReturnType => {
  const pageOptions = reactive<PageType>({
    page: 1,
    size: arg?.size || 10,
    totalElements: 0,
    totalPages: 0,
    first: false,
    last: false,
  });
  const setPageOptions = (page: number, response: PageType) => {
    pageOptions.page = page;
    pageOptions.size = response.size;
    pageOptions.totalElements = response.totalElements;
    pageOptions.totalPages = response.totalPages;
    pageOptions.first = response.first;
    pageOptions.last = response.last;
  };

  return { pageOptions, setPageOptions };
};

```

> 사용 예&#x20;

```typescript
import { usePagination } from "@/hooks/usePagination"; 

export default Vue.extend({ // 이슈 2. 에서 수정!
  setup() {
    const { pageOptions, setPageOptions } = usePagination();
    return { pageOptions, setPageOptions };
  },
  ...
  methods: {
    async getNotice() {
      try {
        const response = await notice_api.getNotice(config);
        if (response.status === 200) {
          const data = response.data.items;
          this.setPageOptions(this.pageOptions.page, data); 
  
```

> 한번에 보여줄 리스트 갯수(size) 조절시 예&#x20;

```typescript
const { pageOptions, setPageOptions } = usePagination({size: 5});
```

###

### 이슈 2. typescript: property does not exist on type 'object & record\<never, any>

* setup() 에서 return한 데이터를 `<template>` 코드 내에서 사용시 기능은 모두 동작하였으나, 위와 같은 빨간줄 typescript 경고가 생겼습니다.
* 테스트해보니 chrome vue extention 에서도 data로 잘 들어오고 있고, build 에러도 뜨지 않았지만, 찾아보니 아래와 같이 수정을 하면 typescript 경고가 발생하지 않았습니다.

#### 참고 자료

* Vue 3로 마이그레이션하기 위해 준비해야 할 것: [https://ui.toast.com/weekly-pick/ko\_20200804](https://ui.toast.com/weekly-pick/ko\_20200804)
  * ``[`defineComponent`를 이용하여 Vue 컴포넌트를 초기화하도록 변경한다.](https://ui.toast.com/weekly-pick/ko\_20200804#%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%EB%A5%BC-%EC%9E%91%EC%84%B1%ED%95%A0-%EB%95%8C-vuecomposition-api%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%9C%EB%8B%A4)

### 해결 코드&#x20;

```typescript
<template>
  ... 
  // 이제 빨간 경고 줄이 생기지 않습니다. 
  <Pagination :pageOptions="pageOptions" @click="onClickPage" />
</template>

import { defineComponent } from "@vue/composition-api";

export default defineComponent({
  setup() {
    const { pageOptions, setPageOptions } = usePagination();
    return { pageOptions, setPageOptions };
  },
```
