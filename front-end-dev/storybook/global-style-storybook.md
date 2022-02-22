---
description: '2021-06-09'
---

# storybook에 Global Style 적용하기

storybook에서 global style이 적용되지 않아서, 실제 스타일과 다른 이슈 발생!

### 해결방법 

> .storybook/preview.js

```text
import GlobalStyle from '../styles/global-styles'; 

export const decorators = [
  Story => (
    <>
      <GlobalStyle />
      <Story />
    </>
  ),
];
```

> styles/global-styles.ts

```text
import { createGlobalStyle } from 'styled-components';
import reset from 'styled-reset';

const GlobalStyle = createGlobalStyle`
  ${reset}
  
  ... global 스타일 적용 
`;
export default GlobalStyle;

```



