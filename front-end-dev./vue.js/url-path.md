---
description: '2021-11-06'
---

# 진입한 url path에 맞게 메서드 실행

### 해결 방법&#x20;

```
beforeRouteEnter(to, from, next) {
    next(vm => {
      if (to.path.indexOf('/path') > -1) {
        vm.methodName;
      }
    })
  },
```



#### 참고자료 &#x20;

* [https://router.vuejs.org/kr/guide/advanced/navigation-guards.html](https://router.vuejs.org/kr/guide/advanced/navigation-guards.html)&#x20;
* [https://stackoverflow.com/questions/53788975/vue-router-how-to-get-previous-page-url](https://stackoverflow.com/questions/53788975/vue-router-how-to-get-previous-page-url)&#x20;
