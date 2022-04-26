---
description: '2022-04-26'
---

# vue+TypeScript

## " **기존 Vue 프로젝트에 TypeScript  적용시키기로 결정! "**

****

인프런 강의 : [https://www.inflearn.com/course/vue-ts](https://www.inflearn.com/course/vue-ts) 에서 추천한 방법인

```
$ vue create project-name
```

로 **TypeScript** 설치 후 기존 코드를 옮기는 방식으로 진행하였다.



> tsconfig.json 에 우선 아래 설정을 추가한 뒤 작업  (작업시 값 변경 예정)

```
"strict": false,
"noImplicitAny": false,
"allowJs": true,
```

### 기본적인 빌드 에러 해결

```
$ npm run build
```

#### 이슈 1. vuejs-logger&#x20;

> 수정 전  ) main.ts&#x20;

```
import VueLogger from "vuejs-logger"; 

Vue.use(VueLogger, options) 
```

> 수정 후  ) main.ts&#x20;

```
import VueLoggerPlugin from "vuejs-logger/dist/vue-logger"; 

Vue.use(VueLoggerPlugin, options)  
```

#### 이슈 2. Vue.$auth, Vue.$log 타입에러

> shims-tsx.d.ts 에 아래 코드 추가&#x20;

```
import { OktaAuth } from "@okta/okta-auth-js";

declare module "vue/types/vue" {
  interface VueConstructor {
    $auth: OktaAuth;
    $log: {
      debug(message: string, trace?: {}): void;
      info(message: string, trace?: {}): void;
      warning(message: string, trace?: {}): void;
      error(message: string, trace?: {}): void;
      fatal(message: string, trace?: {}): void;
    };
  }
}
```

* 참고 : [https://bestofvue.com/repo/justinkames-vuejs-logger-vuejs-inspect-debugging](https://bestofvue.com/repo/justinkames-vuejs-logger-vuejs-inspect-debugging)
