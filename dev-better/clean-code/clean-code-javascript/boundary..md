---
description: 2022-01
---

# 경계 다루기

### min, max (최소값, 최대값)&#x20;

```
genRandomNumber(MIN_NUMBER, MAX_NUMBER);

```

코드만 봐도 명시적으로 함수 내부를 보지 않아도 뭘 의미하는지 알 수 있다.&#x20;

\>, >= 큰 값인지, 크거나 같은 값인지 코딩 컨벤션으로 정하는 게 좋다.&#x20;



### begin, end (시작, 끝)

```
function reservationDate(beginDate, endDate) 
```

예) 예약 사이트 체크인, 체크아웃&#x20;



### first, last (\~부터 \~까지)

연속성이 없고 규칙성이 없을 때 사용&#x20;



### prefix, suffix (접두사, 접미사)&#x20;

코드관리의 일관성을 위해 필요하다.&#x20;

예) 컴포넌트 이름에 Base 붙이기,&#x20;

redux 에서 액션타입 관리시 STARRED\_REQUEST, STARRED\_SUCCESS, STARRED\_FAILURE&#x20;





### 매개변수의 순서가 경계다.

1. 인자는 두개가 넘지 않도록하는게 좋다.
2. 만약에 인자가 많아진다면?

* 매개변수를 객체로 받기
* rest parameter나 arguments로 받기
