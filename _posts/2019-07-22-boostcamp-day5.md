---
layout: post
title: "부스트캠프 챌린지 Day5"
category: [boostcamp, js]
---

1. vscode 디버그 기능 사용법을 익히고, 처음으로 실제로 적용해봤다.
2. js generator function을 사용해봤다.

## **디버깅**

-   **breakpoints란**: 특정 라인이나 함수에 breakpoint를 설정하면, 그곳에서 코드 실행이 멈추고 변수나 scope같은 것들을 확인할 수 있다.
-   **watch사용법**: 코드가 실행되거나 수정됨에 따라 확인하고 싶은 변수명 등을 설정해놓을 수 있다. breakpoint의 위치에 따라 변하는 것을 이용해 디버깅에 유용히 사용할 수 있다.
-   **call stack 의 의미**: 디버거가 멈추기 직전까지 실행됐던 task가 쌓인 것. 실행순서대로 아래부터 위로 쌓인다.
-   **Step over / Step into/ Step out**

---

## **generator function**

함수선언문 뒤에 `*`을 붙여 `function* myFunction() {}`의 형태로 선언할 수 있다. Generator 객체를 반환한다. **javascript의 비동기 프로그래밍**을 위해 사용할 수 있다.

generator function은 한 번에 `return`까지 내려가지 않고, `yield`를 만날때마다 멈추고, 다시 그 자리로 돌아와 실행된다.

```js
function* count(i) {
    const result = yield i;
    yield result + 10;
    yield result + 15;
    return "끝!";
}
const gen = generator(10);
console.log(gen.next());
// { value: 10, done: false }
console.log(gen.next(15));
// { value: 25, done: false }
console.log(gen.next());
// { value: 30, done: false }
console.log(gen.next());
// { value: undefined, done: true }
```

위와 같은 형태로 작성한다.

generator function은 `.next()`가 호출되면 `yield` 구문을 만날 때까지 실행된다. 매번 `.next()`를 호출할 때마다 하나의 yield 구문까지 실행한다.

실행된 이후 `value`와 `done`을 key로 가지는 객체를 반환한다. `value`에는 실행한 결과값이 들어가고, `done`은 실행된 `yield` 구문이 존재하면 `false`, 존재하지 않으면 `true`가 들어간다. `yield`가 실행되지 않으면 함수의 `return` 값이 `value`로 들어간다.

`.next()`안에 인자를 넣어줌으로써 generator fucntion 속 변수로 사용할 수도 있다. 위 코드의 경우 `gen.next(15)`로 `result`에 `15`가 할당되어 25, 30 이라는 결과가 나왔다.
