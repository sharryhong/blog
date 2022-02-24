---
description: '2022-02-24'
---

# Firebase v9 Auth with React

토이프로젝트에 사용한 파이어베이스 관련 코드를 정리해보겠습니다. :thumbsup:

## 파이어베이스 버전9. 인증 setup&#x20;

Firebase 'Go to console'에서 project를 생성합니다.&#x20;

'Add on app to get stated' (web) 클릭 후 app을 등록합니다. &#x20;

app 등록시 config 코드가 나오고, 나중에 확인시에는 왼쪽 상단 '설정'버튼 -> '프로젝트 설정'을 클릭하면 나옵니다.

databaseURL 의 경우 Realtime Database 에서 Create Database

```
// Import the functions you need from the SDKs you need
import { initializeApp } from "firebase/app";
import { getAnalytics } from "firebase/analytics";
// TODO: Add SDKs for Firebase products that you want to use
// https://firebase.google.com/docs/web/setup#available-libraries

// Your web app's Firebase configuration
// For Firebase JS SDK v7.20.0 and later, measurementId is optional
const firebaseConfig = {
  apiKey: "...",
  authDomain: "...",
  databaseURL: "...", 
  projectId: "...",
  storageBucket: "...",
  messagingSenderId: "...",
  appId: "...",
  measurementId: "..."
};

// Initialize Firebase
const app = initializeApp(firebaseConfig);
const analytics = getAnalytics(app);
```

### React Project에 firebase SDK setup하기

코드에 노출되면 안되는 중요한 키값들

관련 링크 : [https://create-react-app.dev/docs/adding-custom-environment-variables/](https://create-react-app.dev/docs/adding-custom-environment-variables/)&#x20;

> .env 예)

```
REACT_APP_FIREBASE_APIKEY=keyvalue
```

> service/firebase.js

```
import { initializeApp } from "firebase/app";

const firebaseConfig = {
  apiKey: process.env.REACT_APP_FIREBASE_APIKEY,
  authDomain: process.env.REACT_APP_FIREBASE_AUTHDOMAIN,
  databaseURL: process.env.REACT_APP_FIREBASE_DATABASEURL,
  projectId: process.env.REACT_APP_FIREBASE_PROJECTID,
};

const firebaseApp = initializeApp(firebaseConfig);

export default firebaseApp;

```

### login, logout 설정 (Google, Github Sign-In)

관련 링크 :  [https://firebase.google.com/docs/auth/web/google-signin?authuser=0](https://firebase.google.com/docs/auth/web/google-signin?authuser=0)&#x20;

로그인, 로그아웃 등 authentication에 관련된 일을 하는 클래스

> service/auth\_service.js

```
import {
  getAuth,
  signInWithPopup,
  GoogleAuthProvider,
  GithubAuthProvider,
} from "firebase/auth";

class AuthService {
  constructor() {
    this.auth = getAuth();
    this.GoogleProvider = new GoogleAuthProvider();
    this.GithubProvider = new GithubAuthProvider();
  }
  login(provider) {
    signInWithPopup(this.auth, this[`${provider}Provider`]);
  }
  logout() {
    this.auth.signOut();
  }
  onAuthStateChanged(onChangedUser) {
    this.auth.onAuthStateChanged((user) => {
      onChangedUser(user);
    });
  }
}

export default AuthService;

```

### firebase config 와 연결 후 dependency injection

> index.js

```
import firebaseApp from "./service/firebase.js";
import AuthService from "./service/auth_service.js";

// firebase config 와 연결 
const authService = new AuthService(firebaseApp);

ReactDOM.render(
  <React.StrictMode>
    <BrowserRouter>
        // dependency injection 
        <App authService={authService} />
    </BrowserRouter>
  </React.StrictMode>,
  document.getElementById("root")
);
```

### login , logout 호출

> App.jsx

```
const onLogin = (event) => {
  authService.login(event.target.innerText);
};

const onLogout = useCallback(() => {
  authService.logout();
  navigate("/");
}, [authService]);
```



