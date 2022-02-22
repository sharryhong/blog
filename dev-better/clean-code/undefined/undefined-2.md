---
description: 2022-01
---

# Conditional. 분기 다루기

### 값식문

React의 JSX는 babel에 의해 js로 트랜스파일링되면 객체로 바뀐다.&#x20;

따라서 JSX에 '문'이 오면 안되고, '값'이나 '식만 넣어야 한다. \


예) 삼항연산자는 값으로 귀결되므로 사용가능. if문은 '문'이므로 사용하면 안됨&#x20;

### &#x20;삼항연산자&#x20;

과도한 숏코딩은 가독성이 떨어진다. if문이나 switch case문이 나을 수 있다.

> 좋은 예) null일 수 있는 상황에 맞게 쓰여짐

```
const welcomeMessage = (isLogin) => {
  const name = isLogin ? getName() : '이름없음';
  return `안녕하세요 ${name}`;
};
```

> 나쁜 예) 값또는 식이 나와야 하는데 함수를 실행시키고 있다. 둘다 undefinded일 수 있다.

```
function alertMessage(isAdult) {
  isAdult
    ? alert('입장 가능')
    : alert('입장 불가');
}
```

### 단축평가(short-circuit evaluation)&#x20;

&& 연산자: 모두가 true여야 마지막까지 도달&#x20;

|| 연산자 : 모두가 false여야 마지막까지 도달&#x20;

> 수정 전 코드&#x20;

```
function getActiveUserName(user, isLogin) {
  if(isLogin) {
    if(user) {
      if(user.name) {
        return user.name
      } else {
        return '이름없음'
      }
    }
  }
}
```

> 단축평가 사용으로 개선 코드&#x20;

```
function getActiveUserName(user, isLogin) {
  if(isLogin && user) {
    return user.name || '이름없음'
  }
}
```

### Early Return

> Early Return 사용으로 읽기 쉬워진 코드 예) &#x20;

```
function loginService(isLogin, user) {
  if (login) { 
    return; 
  }
  if (!user.nickName) {
    return registerUser(user);
  }
  ...
}
```

### ?? Nulish coalescing operator (널 병합 연산자)

[https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Nullish\_coalescing\_operator](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Nullish\_coalescing\_operator) \


a || b 인 경우 a가 falsy면 b 실행

0, '' 도 false가 된다.



**널 병합 연산자 (??) 로 변경하면 null, undefined일때만 평가된다.**

a ?? b



optionam chaining operator와 함께쓰면 유용하다. [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Nullish\_coalescing\_operator#optional\_chaining\_연산자.와의\_관계](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Nullish\_coalescing\_operator#optional\_chaining\_%EC%97%B0%EC%82%B0%EC%9E%90.%EC%99%80%EC%9D%98\_%EA%B4%80%EA%B3%84)
