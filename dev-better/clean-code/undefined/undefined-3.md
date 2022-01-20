---
description: 2022-01
---

# 배열 다루기

### JS의 배열은 객체이다.&#x20;

배열은 객체이므로 주의가 많이 필요하다.&#x20;

배열임을 확인하고 싶을때는 Array.isArray(arr); 를 활용하자. &#x20;

### Array.length

length는 배열의 길이보다는, 배열의 마지막 index라고 생각하자.&#x20;

arr.length = 0; 으로 하면 배열이 초기화된다.&#x20;

주의해서 사용하자.&#x20;

### 배열 요소에 접근하기

\[0], \[1] 으로 요소에 접근하는 코드를 지양하자.

input\[0], input\[1] 의 경우 &#x20;

> 개선코드 예 1)

```
const [firstInput, secondInput] = input; 
```

> 개선코드 예2) 함수에서 받을 때부터 분해하기

```
function operateTime([firstInput, secondInput].. ) { 
  ... 
} 

operateTime([1, 2],...) 
```

> 개선코드 예3) 유틸함수 만들어 사용하기. lodash의 \_.head(array) // 첫번째 요소 얻기

```
function head(arr) {
  return arr[0] ?? '없을때처리'
}

function formatDate(targetData) {
  const date = head(targetDate.toISOString().split('T'));
  const [year, month, day] = date.split('-');

  return `${year}년  ${month}월 ${day}일`;
}
```



