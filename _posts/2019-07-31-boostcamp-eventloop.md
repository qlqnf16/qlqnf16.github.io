---
layout: post
title: "부스트캠프 챌린지 Day11"
tags: [boostcamp, js]
---

js의 핵심, callstack과 eventloop

callstack과 eventloop에 대해 알아보고, 직접 구현해보았다. 그동안 동기/비동기 처리를 할 때 거의 감에(..) 의존해서 코딩했었는데, 이 미션을 통해 브라우저에서 js가 작동하는 방식을 더 제대로 이해할 수 있었다.

이벤트루프를 이해하고 싶다면: [What the Heck is Event Loop](https://www.youtube.com/watch?v=8aGhZQkoFbQ)

## EventQueue, CallStack 구현

```js
function executeEventQueue(callStack, eventQueue) {
  setInterval(() => {
    if (callStack.length == 0) {
      if (eventQueue.length > 0) callStack.push(eventQueue.shift());
    }
  }, 0);
}

function executeCallStack(callStack, startTime) {
  let endTime = new Date().getTime();
  if (endTime - startTime >= 5000) {
    process.exit();
    return;
  }

  while (callStack.length > 0) {
    let cb = callStack.pop();
    cb();

    startTime = new Date().getTime();
  }

  setTimeout(executeCallStack, 0, callStack, startTime);
}
```

## Call stack / Event Queue / Event loop 설명

- call stack
  - 프로그램에서 함수가 실행되는 순서대로 함수를 저장하는 Stack이다. 함수가 호출되면 stack에 쌓이고 `return` 되면 stack에서 제거된다. LIFO에 따라 제일 뒤에 있는 함수부터 차례로 실행, 제거된다.
- event queue
  - javascript 비동기프로그래밍에서 브라우저 API에 의해 콜백 함수들이 저장되는 곳이다.
  - event loop이 callstack이 비었음을 알리면 실행된다
- event loop
  - **event loop**는 call stack과 event queue를 감시하다가 call stack이 비면 그 즉시 event queue에 대기중인 콜백함수를 FIFO로 실행한다.
- micro task queue(== ECMA2015::job queue)
  - 프로미스를 지원하기 위해서 나온 새로운 큐. html스펙에서는 'micro task queue'라고 정의되어있고 ECMA2015에서는 'job queue'라고 정의되어 있다.
