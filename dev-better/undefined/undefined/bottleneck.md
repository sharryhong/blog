---
description: 2022-02
---

# bottleneck (병목) code 탐색

느린 js 코드를 최적화 해보자.

<mark style="color:green;">Lighthouse 툴</mark> 결과에서&#x20;

* <mark style="color:green;">Minimize main-thread work</mark> : 메인스레드 작업을 줄여라.
* <mark style="color:green;">Reduce JavaScript execution time</mark> : js의 실행시간을 줄여라. 각 파일당 소요 시간이 나오지만 어느 코드에서 문제가 있는지는 알 수 없다. &#x20;

### Perfomance 탭&#x20;

페이지가 로드되면서 실행되는 작업들을 타임라인과 챠트 형태로 보여준다.

참고 링크 : [https://developer.chrome.com/docs/devtools/evaluate-performance/](https://developer.chrome.com/docs/devtools/evaluate-performance/)



