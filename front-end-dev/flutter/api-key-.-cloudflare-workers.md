---
description: '2023-06-28'
---

# API Key 안전하게 숨기기. Cloudflare Workers!

### 이슈

현재 서드파티 API로 Youtube Data API를 사용중입니다.&#x20;

api호출시 key가 네트워크에 노출되고 있었는데요,\
Cloudflare Workers 서비스로 기존코드를 거의 수정할 필요 없이, 거의 무료로 사용할 수 있다는걸 알게되었습니다.&#x20;

### 해결 방법&#x20;

자료에 링크해놓은 니콜라스 유튜브를 참고하여&#x20;

1. Cloudflare Workers 계정만들기 [https://workers.cloudflare.com/](https://workers.cloudflare.com/)&#x20;
2. 설치 `npm install -g wrangler`
3. 로그인 `wrangler login`&#x20;
4. 프로젝트에 keys 폴더 생성 `wrangler init keys -y`
5. keys 폴더로 이동 후 dev시 사용할 url 얻기  `wrangler dev`&#x20;
6. 배포후 사용할 url 얻기 `wrangler deploy`
7. 실제 api 요청해보기&#x20;

현재 플러터에서 api관련 코드는 lib/services/api\_service.dart에서 관리중입니다.&#x20;

```dart
...
class ApiService {

  static const String baseUrl = kReleaseMode
      ? "https://.." // wrangler deploy시 나온 url 
      : "http://.."; // wrangler dev시 나온 url 
      
  ...

  static Future<List<SearchItemModel>> getSearchItems(String text,
      [int? max]) async {
    ...
    final url = Uri.parse(
        '$baseUrl/search?part=snippet&maxResults=$maxResults&q=$text&type=video');
 
    final response = await http.get(url);

    if (response.statusCode == 200) {
      return ...;
    }

```

* 기존 코드에서 수정한 부분은 baseUrl부분, 그리고 Uri.parse에서 api key 부분을 삭제한 것 뿐입니다.&#x20;
* 즉, 서드파티 api 로 직접 호출하지 않고  Cloudflare Workers 를 거쳐서 호출하게 됩니다.&#x20;



위 4번에서 keys폴더 내에 생성된 worker.ts파일

```typescript
...
export default {
  async fetch(request: Request, env: Env, ctx: ExecutionContext): Promise<Response> {
    const baseUrl = '서드파티 url';
    const apiKey = 'apiKey';

    const { pathname, searchParams } = new URL(request.url);

    const relayUrl = `${baseUrl}${pathname}?key=${apiKey}&${searchParams.toString()}`;
    const response = await fetch(relayUrl);

    const json = await response.json();
    const jsonString = JSON.stringify(json);

    return new Response(jsonString, {
      status: response.status, 
      headers: {
        'Content-Type': 'application/json; charset=utf-8',
        'Access-Control-Allow-Origin': '*',
        'Access-Control-Allow-Methods': 'GET, POST, OPTIONS',
      },
    });
  },
};
```

* status: response.status // ApiService에서 response.statusCode 를 사용할 수 있습니다.&#x20;
* charset=utf-8 // 한국어가 깨지지 않습니다.&#x20;

#### 자료

* [땡큐 니콜라스! ](https://www.youtube.com/watch?v=wAWOOBUAclc)

