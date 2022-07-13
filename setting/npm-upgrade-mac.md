---
description: '2021-05-25'
---

# npm upgrade (mac)

### 이슈&#x20;

npm 설치가 되지 않음&#x20;

지금까지 yarn 으로 패키지 관리를 하였는데, 이번 프로젝트에서 npm으로 관리해보자고 누군가가 의견을 내었습니다. \
node와 npm의 버전을 맞추어 작업하기로 하고, node는 안정적인 LTS버전으로, npm은 최신버전으로 설치

### 해결방법&#x20;

1. 홈브루 업그레이드&#x20;

```
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

2\. npm 설치

```
$ sudo npm install -g npm 
```

#### 참고링크&#x20;

* 궁금하여 찾아본 > npm vs yarn [https://dongmin-jang.medium.com/npm-vs-yarn-7b699c5d6034](https://dongmin-jang.medium.com/npm-vs-yarn-7b699c5d6034) \
  \=> 보안 이슈는 npm 6버전부터 보완이 되었다고한다.&#x20;
* 패키지매니저별 성능지표 [https://pnpm.io/benchmarks](https://pnpm.io/benchmarks)&#x20;

