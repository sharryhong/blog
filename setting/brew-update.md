---
description: '2022-10-26'
---

# brew update

### 이슈&#x20;

homebrew core is a shallow clone. 에러가 뜨며 홈브루 업데이트가 되지않는 이슈



### 해결 방법

터미널에 아래 명령어를 실행합니다. 시간이 꽤 오래걸립니다.&#x20;

```
$ git -C /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core fetch --unshallow
```

검색해보면 이 명령어 말고도 다른 것들도 나오는데 제가 해보니 이 것만 실행해도 잘 진행되었습니다.&#x20;

```
$ brew update 
```

이제 홈브루 업데이트가 잘 진행됩니다.&#x20;



#### 참고 자료&#x20;

[https://github.com/Homebrew/discussions/discussions/226](https://github.com/Homebrew/discussions/discussions/226)&#x20;
