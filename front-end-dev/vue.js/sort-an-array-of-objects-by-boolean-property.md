---
description: '2022-07-14'
---

# Sort an Array of Objects by Boolean property

### 이슈&#x20;

#### sort 기준

* 1 순위 : isBookmarked === true 이고 isAble === true
* 2 순위 : isBookmarked === true 이고 isAble === false
* 3 순위 : isAble === true \


기존코드는 위의 위의 정렬 기준에 따라 if, else if를 사용하여 임시변수에 담는 방식으로 개발되어있었는데, &#x20;

JavaScript Array 내장 메소드인 sort()를 사용하며 리팩토링 해보았습니다. &#x20;

#### 참고 자료

* [https://bobbyhadz.com/blog/javascript-sort-array-of-objects-by-boolean-property](https://bobbyhadz.com/blog/javascript-sort-array-of-objects-by-boolean-property)
* [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global\_Objects/Array/sort](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global\_Objects/Array/sort)&#x20;

### 해결 코드

```
list
  .sort((a, b) => Number(b.isAble) - Number(a.isAble))
  .sort((a, b) => Number(b.isBookmarked) - Number(a.isBookmarked));
```

true = 1, false = 0 으로 값을 변경하여,&#x20;

a - b 는 오름차순으로 정렬하므로, b - a 로 해야 true가 앞에 놓이게 됩니다.&#x20;

`Number(b.isAble) - Number(a.isAble)  // isAble === true` 인 순서대로 정렬시킵니다.&#x20;

그 후 `Number(b.bookmark) - Number(a.bookmark) // isBookmarked === true` 인 순서로 정렬시키므로,&#x20;

위에 명시한 sort 기준대로 정렬되게 됩니다.&#x20;
