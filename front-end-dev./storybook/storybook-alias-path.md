---
description: '2021-05-13'
---

# storybook에서 alias path 안되는 이슈

상대경로 대신 @/\* 로 사용하기 위해 tsconfig.json 파일에 아래와 같이 셋팅을 하고 작업중입니다.&#x20;

```
"paths": {
      "@/components/*": ["components/*"],
      "@/pages/*": ["pages/*"],
      "@/styles/*": ["styles/*"],
      "@/utils/*": ["utils/*"]
    }            
```

그런데! storybook에서는 \`import xxx from '@/components/xxx';\` 와 같은 경로가 인식되지 않는 이슈발생&#x20;

### 해결방법&#x20;

npm install : [tsconfig-paths-webpack-plugin ](https://www.npmjs.com/package/tsconfig-paths-webpack-plugin)

> .storybook/main.js

```
const { TsconfigPathsPlugin } = require('tsconfig-paths-webpack-plugin');

module.exports = {
  stories: ['../components/**/*.stories.mdx', '../components/**/*.stories.@(js|jsx|ts|tsx)'],
  addons: [
    '@storybook/addon-links',
    '@storybook/addon-essentials',
    'storybook-addon-styled-component-theme/dist/preset',
  ],

  webpackFinal: async (config) => {
    [].push.apply(config.resolve.plugins, [
      new TsconfigPathsPlugin({ extensions: config.resolve.extensions })
    ]);

    return config;
  }
};
```
