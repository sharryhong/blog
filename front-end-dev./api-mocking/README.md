---
description: '2021-05-26'
---

# API Mocking

회사 기술세미나에서 보고 바로 접목해보고 있는 [MSW.js](https://mswjs.io/) 에 대해 소개합니다. 

> 백엔드에서 API가 개발되기 전에, 임의의 json파일을 만들어 프론트엔드에서 작업하고 있었다.   
> 이렇게 작업 후 실제 백엔드 api로 대응시, 서비스로직의 수정이 필요하거나, 네트워크 수준에서 \(http method, 응답상태 등\)의 mocking은 어렵다는 이슈가 있었다.

### 위의 이슈를 보완하기 위한 MSW\(Mock Service Worker\) library

* 브라우저의 백그라운드 스레드로 동작하여 UI Blocking 없이 실행 
* 네트워크 http request 를 intercept하여 동작 



#### 참고 자료

* [https://mswjs.io/docs/](https://mswjs.io/docs/) 
* [https://www.npmjs.com/package/msw](https://www.npmjs.com/package/msw)
* react : [https://github.com/mswjs/examples/tree/master/examples/rest-react](https://github.com/mswjs/examples/tree/master/examples/rest-react)
* next : [https://github.com/vercel/next.js/tree/canary/examples/with-msw](https://github.com/vercel/next.js/tree/canary/examples/with-msw)
* vue : [https://www.vuemastery.com/blog/mock-service-worker-api-mocking-for-vuejs-development-testing/](https://www.vuemastery.com/blog/mock-service-worker-api-mocking-for-vuejs-development-testing/) 



