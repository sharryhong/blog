---
description: 2022-04
---

# React App deploying (배포)

## 배포 단계

1. Test Code
2. Optimize Code (최적화) : 예) lazy loading
3. Build app for production : 프로덕션용 앱 빌드하기 (bundle, minify)
4. Upload production code to Server&#x20;
5. Configure Server : 서버 구성. 호스팅 제공업체 구성



### 3. Build app for production : 프로덕션용 앱 빌드하기

```
$ npm run build  
```

최종 deploy 파일들이 build 폴더에 생성되었다.&#x20;

빌드할 때마다 덮어 씌우므로 수정하지 않도록 한다.&#x20;



### 4. Upload production code to Server&#x20;

React SPA 는 Static Website로 서버사이드 코드가 없다. 따라서 static side host가 필요하다.&#x20;

구글에 static website hosting 이라고 검색해보자.&#x20;

이번엔 Firebase hosting을 사용해보았다.&#x20;



Firebase 콘솔로 이동하여 왼쪽 메뉴에서 'Hosting'을 선택 후 '시작하기' 버튼을 클릭한다.



그 후 나오는 설정대로&#x20;

#### 1. Firebase CLI 설치&#x20;

```
$ sudo npm install -g firebase-tools 
```

#### 2. 프로젝트 초기화&#x20;

```
firebase login
```

```
firebase init 
```

질문&#x20;

#### ? What do you want to use as your public directory? build&#x20;

* (중요) build 폴더를 사용해야하므로 build라고 입력한다. &#x20;

#### ? Configure as a single-page app (rewrite all urls to /index.html)? Yes&#x20;

* 모든 url을 index.html로 다시 작성할 것인가. Yes 선택.&#x20;
* &#x20;url경로가 서버에서는 무시되고 항상 spa 코드를 반환하도록 한다.&#x20;

#### ? Set up automatic builds and deploys with GitHub? No

* 자동 빌드, 배포가 필요한가&#x20;

#### ? File build/index.html already exists. Overwrite? No&#x20;



파이어베이스 배포 구성 완료되었다.&#x20;



#### 3. Firebase 호스팅에 배포&#x20;

```
$ firebase deploy 
```

이제 배포되었다.&#x20;

콘솔로가면 custom domain 추가 가능  &#x20;



호스팅 비활성화

```
$ firebase hosting:disable  
```
