---
description: '2022-08-19'
---

# usePagination - composition api

### 이슈&#x20;

Pagination 컴포넌트를 사용하는 곳에서 공통으로 사용하는 코드를 composition api 를 사용하여 코드를 개선하고자 하였습니다.&#x20;

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

> usePagination.ts

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

export default Vue.extend({
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
