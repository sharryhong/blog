---
description: '2022-01-25'
---

# 절대경로(absolute path) 설정, jsx파일로 이동 설정

이슈 : 절대경로로 설정 후 사용 중 mac의 경우 option key + 컴포넌트명 클릭시 파일로 이동하지 않는 이슈 발생



### 절대경로 설정

> jsconfig.json

```
{
  "compilerOptions": {
    "baseUrl": "src"
  },
  "includes": ["src"]
}
```

> 사용 예)

```
import Header from "pages/layout/header/Header";
```

```
@value makerGreen, makerWhite from 'common/colors.css'; 
```



### jsx파일로 이동

jsconfig.json 에 "jsx": "react",  추가&#x20;

> jsconfig.json

```
{
  "compilerOptions": {
    "jsx": "react",
    "baseUrl": "src"
  },
  "includes": ["src"]
}
```

참고링크: [https://github.com/microsoft/vscode/issues/30290](https://github.com/microsoft/vscode/issues/30290) &#x20;
