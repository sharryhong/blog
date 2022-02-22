---
description: '2021-08-24'
---

# input file 선택된 파일 명 reset하기

이슈 : vuetify v-file-input 컴포넌트 사용 중 파일 선택 후, 다른 곳에서 리셋을 하는 기능이 있다. 파일관련 데이터가 삭제되어도 v-file-input 상의 file name 은 남아있는 이슈&#x20;

참고링크 : [https://stackoverflow.com/questions/63446829/how-to-reset-vuetifys-v-input-file](https://stackoverflow.com/questions/63446829/how-to-reset-vuetifys-v-input-file)&#x20;

해결 방법 : key를 추가한다.&#x20;

```
<v-file-input
  :key="fileKey"
  @change="handleChangeFile"
></v-file-input>
...

data() {
    return {
      ...
      fileKey: 0,
    }
  },
methods: {
    resetDatas() {
      ...
      this.fileKey++;
    },
```

