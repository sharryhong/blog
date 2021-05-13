---
description: '2021-05-12'
---

# styled-component theme.ts가 storybook엔 적용안되는 이슈

### 해결방

#### npm install : [https://www.npmjs.com/package/storybook-addon-styled-component-theme](https://www.npmjs.com/package/storybook-addon-styled-component-theme) 

> .storybook/preview.js

```text
import { addDecorator } from '@storybook/react';
import { withThemesProvider } from 'storybook-addon-styled-component-theme';
import { ThemeProvider } from 'styled-components';
import theme from '../styles/theme'; // theme.ts 파일 

const themes = [theme];
addDecorator(withThemesProvider(themes), ThemeProvider);

export const parameters = {
  actions: { argTypesRegex: "^on[A-Z].*" },
  controls: {
    matchers: {
      color: /(background|color)$/i,
      date: /Date$/,
    },
  },
}
```

> .storybook/main.js

```text
module.exports = {
  stories: ['../components/**/*.stories.mdx', '../components/**/*.stories.@(js|jsx|ts|tsx)'],
  addons: [
    '@storybook/addon-links',
    '@storybook/addon-essentials',
    'storybook-addon-styled-component-theme/dist/preset', // 추가 
  ],
};
```



