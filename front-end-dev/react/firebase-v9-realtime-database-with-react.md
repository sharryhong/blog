---
description: '2022-02-24'
---

# Firebase v9 Realtime Database with React

토이프로젝트에 사용한 파이어베이스 관련 코드를 정리해보겠습니다. :tada:

## 파이어베이스 버전9. Realtime Database setup

관련  링크  &#x20;

* [https://firebase.google.com/docs/database/web/start](https://firebase.google.com/docs/database/web/start)&#x20;
* [https://firebase.google.com/docs/database/web/read-and-write](https://firebase.google.com/docs/database/web/read-and-write)&#x20;

firebase docs 코드&#x20;

> Write data

```
import { getDatabase, ref, set } from "firebase/database";

function writeUserData(userId, name, email, imageUrl) {
  const db = getDatabase();
  set(ref(db, 'users/' + userId), {
    username: name,
    email: email,
    profile_picture : imageUrl
  });
}
```

> Read data

```
import { getDatabase, ref, onValue} from "firebase/database";

const db = getDatabase();
const starCountRef = ref(db, 'posts/' + postId + '/starCount');
onValue(starCountRef, (snapshot) => {
  const data = snapshot.val();
  updateStarCount(postElement, data);
});
```

### React Project에 적용하기

> service/card\_service.js

```
import firebaseApp from "./firebase";
import { getDatabase, ref, set, onValue, remove } from "firebase/database";

const database = getDatabase(firebaseApp);

export const saveCard = (userId, card, onUpdate) => {
  const url = `${userId}/cards/${card.id}`;
  set(ref(database, url), {
    ...card,
  });
  onUpdate(card);
};

export const deleteCard = (userId, card, onUpdate) => {
  const url = `${userId}/cards/${card.id}`;
  remove(ref(database, url));
  onUpdate(card);
};

export const syncCard = (userId, onUpdate) => {
  const url = `${userId}/cards`;
  const starCountRef = ref(database, url);
  onValue(starCountRef, (snapshot) => {
    const data = snapshot.val();
    data && onUpdate(data);
  });
};
```

코드 사용 예

```
import * as cardApi from "service/card_service"; 

cardApi.syncCard(userId, (cards) => {
  dispatch({ type: "SYNC", payload: cards });
});
```
