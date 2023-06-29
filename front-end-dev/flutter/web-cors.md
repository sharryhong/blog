---
description: '2023-06-13'
---

# Web CORS 에러

### 이슈

Youtube Data API 사용시 응답값 중 썸네일 이미지 CORS 에러

확인해보니 ios, 안드로이드에서는 잘 나오는데, pc web 크롬, 사파리에서 CORS 에러가 나오고 있었습니다.&#x20;



### 해결 방법&#x20;

빌드시 아래 명령어로! :tada:

```bash
flutter build web --web-renderer html --release 
```



구글링 해서 여러 방법으로 테스트 해보았는데요

다른 방법은 크롬에서는 되고 사파리에서는 안되는 등의 이슈가 있었습니다.&#x20;

저 방법으로 할 경우, 개발 중일때는 이미지가 cors에러가 나지만 빌드 후 배포하면 이미지가 잘 나옵니다.&#x20;

개발 중일 때도 확인해보려면 아래 명령어를 입력하시면 됩니다. :thumbsup:

```
flutter run -d chrome --web-renderer html 
```

#### 자료&#x20;

[https://velog.io/@thovy/TROUBLESHOOTING-Flutter-web-CORS](https://velog.io/@thovy/TROUBLESHOOTING-Flutter-web-CORS)&#x20;





