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

### 유사 배열 객체

예 : 함수 내에서 사용하는 arguments, querySelectAll 엘리먼트 리스트

진짜 배열이 아니기때문에 내부 프로토타입에 배열 내장함수가 없다.

배열로 바꿔준 후 사용한다.

```
Array.from(arguments).map((arg) => ...)
```

### 불변성 (immutable)

원본배열의 불변성을 지키기 위해서는

> 방법 1. 배열을 복사한다.

```
function copyArray(originArray) {
  const newArray = [ ...originArray ];
  return newArray;
}
```

> 방법 2. 새로운 배열을 반환하는 메서드들을 활용한다.

```
filter(), map(), slice() 등 
```

push, unshift등의 함수는 원본 배열을 바꾼다.



### for문 배열 고차함수로 리팩토링

> 임시변수 사용코드

```
const price = [1000,2000,3000];

function getWonPrice(priceList) {
  let temp = [];
  for (let i = 0; i < priceList.length; i++) {
    temp.push(priceList[i] + '원');
  }
  return temp;
}
```

> map 배열 고차함수를 이용해서 선언, 명시적으로 리팩토링

```
const price = [1000,2000,3000];

function getWonPrice(priceList) {
  return priceList.map((price) => price + '원');
}
```



### 배열 메서드 체이닝 활용하기

> 체이닝 활용 전 코드

```
const price = [5000,2000,1000,3000,1500];

const suffixWon = (price) => price + '원';
const isOverOneThousand = (price) => price > 1000;
const ascendingList = (a, b) => a - b;

function getWonPrice(priceList) {
  const isOverList = priceList.filter(isOverOneThousand);
  const sortList = isOverList.sort(ascendingList);
  
  return isOverList.map(suffixWon)
}
```

> 메서드 체이닝 활용 : 명시적, 앞으로 계속 코드가 추가되어도 명확하게 작성할 수 있다.

```
const price = [5000,2000,1000,3000,1500];

const suffixWon = (price) => price + '원';
const isOverOneThousand = (price) => price > 1000;
const ascendingList = (a, b) => a - b;

function getWonPrice(priceList) {
  return priceList
    .filter(isOverOneThousand)
    .sort(ascendingList)
    .map(suffixWon);
}
```
