---
description: '2022-03-02'
---

# React Star Rating (별점 표시)

star.svg 를 사용하여 별점표시 컴포넌트를 만들어보았습니다.

개발환경 : React, Typescript



> src/assets/star.svg // 별 하나&#x20;

```
<svg xmlns="http://www.w3.org/2000/svg" width="15" height="15" viewBox="0 0 24 24">
  <path d="M12 .587l3.668 7.568 8.332 1.151-6.064 5.828 1.48 8.279-7.416-3.967-7.417 3.967 1.481-8.279-6.064-5.828 8.332-1.151z"/>
</svg> 
```

> StarRating.tsx&#x20;

```
import React from "react";
import { ReactComponent as Star } from "assets/star.svg";

interface RatingType {
  rating?: number;
  maxRating?: number;
}

const StarRating = ({ rating, maxRating = 5 }: RatingType) => {
  return (
    <>
      {[...Array(maxRating)].map((star, i) => {
        if (rating && i < rating) {
          return <Star key={`${star}${i}`} fill="#ffc500" />;
        }
        return <Star key={`${star}${i}`} fill="#ccc" />;
      })}
    </>
  );
};

export default StarRating;

```

*   props로 받은 rating 만큼 노란색 (#ffc500)으로 표시하고,&#x20;

    나머지는 회색(#ccc)으로 표시합니다.&#x20;
* maxRating 는 별 갯수로, 상위컴포넌트에서 props로 주지 않으면 기본값 5개로 셋팅됩니다.

> 사용시 예&#x20;

```
<StarRating rating={rating} /> 
```
