---
description: '2022-02-22'
---

# useContext 리렌더링 이슈

이슈 : memo, useCallback 사용 후에도 리렌더링 되 이슈발생. 이유를 찾아보니 useContext 때문이었다.



참고링크&#x20;

* [https://www.nielskrijger.com/posts/2021-02-16/use-reducer-and-use-context/](https://www.nielskrijger.com/posts/2021-02-16/use-reducer-and-use-context/)
* [https://blog.axlight.com/posts/4-options-to-prevent-extra-rerenders-with-react-context/](https://blog.axlight.com/posts/4-options-to-prevent-extra-rerenders-with-react-context/)&#x20;



위의 참고 자료 중에 해결한 방법

### state와 dispatch context를 분할하여 사용

> 수정 전 코드  &#x20;

```
import React, { createContext, useReducer } from "react";
import { MakerReducer } from "./maker_reducer";

export const MakerContext = createContext();

const initState = { ... };

const MakerStore = ({ children }) => {
  const [cards, dispatch] = useReducer(MakerReducer, initState);

  return (
    <MakerContext.Provider value={{ cards, dispatch }}>
      {children}
    </MakerContext.Provider>
  );
};
```

> 수정  코드

```
import React, { createContext, useReducer } from "react";
import { MakerReducer } from "./maker_reducer";

export const MakerStateContext = createContext();
export const MakerDispatchContext = createContext();

const initState = { ... };

const MakerStore = ({ children }) => {
  const [cards, dispatch] = useReducer(MakerReducer, initState);

  return (
    <MakerDispatchContext.Provider value={dispatch}>
      <MakerStateContext.Provider value={cards}>
        {children}
      </MakerStateContext.Provider>
    </MakerDispatchContext.Provider>
  );
};

```

> 사용시

```
import { MakerStateContext } from "store/maker_store";

const cards = useContext(MakerStateContext);
```

```
import { MakerDispatchContext } from "store/maker_store";

const dispatch = useContext(MakerDispatchContext);
```
