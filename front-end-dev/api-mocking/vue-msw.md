---
description: '2021-08-06'
---

# Vue 에서 msw 적용하기

참고자료 : [https://www.vuemastery.com/blog/mock-service-worker-api-mocking-for-vuejs-development-testing/](https://www.vuemastery.com/blog/mock-service-worker-api-mocking-for-vuejs-development-testing/) 

이미 작업이 시작된 Vue 프로젝트. 백엔드 개발 일정이 미뤄지면서 msw를 적용하기로 하였습니다.   
전에는 react에 초반 작업만 적용해보았는데 이번엔 vue! 크게 다르지 않게 사용이 가능하군요 :\)

위의 참고자료와 동일하게 구성하며 

> .env

```text
VUE_APP_API_MOCKING = enabled
```

dev에서도 켜고 끌 수 있도록 이렇게 설정하였습니다. 



추가 작업으로는 

> /src/mocks/handlers.js

```text
import { rest } from 'msw'

export default [
  rest.get('/admin/rdfizer/history', (req, res, ctx) => {
    return res(
      ctx.status(200),
      ctx.delay(500),
      ctx.json({
        list: [
          { ...
```

이렇게 

ctx.status\(200\) : [https://mswjs.io/docs/api/context/status](https://mswjs.io/docs/api/context/status) 

ctx.delay\(500\) : [https://mswjs.io/docs/api/context/delay](https://mswjs.io/docs/api/context/delay) 

필요한 상황을 추가하며 실제 백엔드 api 호출하는 것처럼 다룰 수 있어서 좋았습니다. 

