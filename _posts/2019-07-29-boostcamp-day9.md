---
layout: post
title: "부스트캠프 챌린지 Day9"
category: [boostcamp, js, testing]
---

테스팅, 테스팅, 말로만 듣던 테스팅을 드디어 해 보았다. 테스팅은 단순하게 말하면, 각각의 함수가 원하는 결과값을 도출하는지 확인하는 단위테스트를 통해 오류를 사전에 검출하고 프로그램의 완성도를 높이는 작업이다. 이번 미션에서는 아주 간단한 테스터를 직접 구현해봤는데, 사실 아직까지 프론트엔드에서 테스팅의 필요성이 잘 와닿지는 않는다. 하다보면 알겠지..

# **테스팅**

주어진 기본 구조는 실제 동작하는 함수가 있는 `source.js`, `source.js`를 테스트하기 위한 `source.test.js`, 그리고 테스팅 라이브러리의 역할을 하는 `tester.js`로 구성되어 있다.

## **첫 코드**

`test(message, func)`함수는 테스트 메세지와 assert 함수를 각각 인자로 받는다. assert에 속한 equal, notEqual, detailEqual 등의 함수를 구현하는 것이 중요 이슈였다. 처음에는 각각의 조건에 맞는 값을 return 하는 방식으로 시도했다.

```js
const test = (message, func) => {
  let result = "";
  const answer = func()           			 // answer: undefined
  result = answer ? 'PASS' : 'FAIL'
  console.log(`${message} : ${result}`);
};

const assert = {
  equal: (actual, expected) => (actual === expected)
  ...
}
```

그러나 이렇게 할 경우, test 함수 내에서 assert.eqaul 의 `return` 값을 정상적으로 받을 수 없다. (함수는 실행되는데 계속 `undefined`가 나온다…)

## **코드 개선**

그래서 참일 경우를 return 하는 것이 아닌, 거짓일 경우 **Throw error**를 해주고, test 함수 내에서 **try**와 **catch**로 구분하는 방식으로 수정했다.

```js
const test = (message, func) => {
  let result = "";
  try {
    func();
    result = "\x1b[34mPASS\x1b[37m";
  } catch {
    result = "\x1b[31mFAIL\x1b[37m";
  }
  console.log(`${message} : ${result}`);
};

const assert = {
  equal: (actual, expected) => {
    if (actual !== expected) throw new Error("not Equal");
  }
  ...
}
```

위와 같이 코드를 구성할 경우, equal 함수 내에서 `actual === expected` 라면 정상적으로 실행이되므로 try 구문 내에서 result 에 'PASS'가 할당되고, 반대로 `actual !== expected` 조건에 걸리면 그 즉시 에러를 Throw 하고 catch 구문으로 넘어가 result에 'FAIL'이 할당된다.

에러를 throw 하는 방식을 통해 예외가 등장하는 순간 바로 함수를 종료시키고 빠져나오므로, 더 효율적으로 작동하기도 할 것이라는 생각이 들었다.

```js
detailEqual: (actual, expected) => {
    if (actual.length !== expected.length) throw new Error();
    else {
        for (let i = 0; i < actual.length; i++) {
            if (typeof actual[i] !== typeof expected[i]) throw new Error();
            if (
                Array.isArray(actual[i]) &&
                !assert.detailEqual(actual[i], expected[i])
            )
                throw new Error();
            if (actual[i] !== expected[i]) throw new Error();
        }
    }
};
```

detailEqual 함수의 경우, `[1, 2, [3, 4], 5]` 와 같이 중첩된 배열을 비교해야 하므로 재귀적으로 구현해야 했다. 따라서 이 과정에서 배열의 길이가 다르거나, 타입이 다른 등 예외가 나오면 즉시 함수를 종료시켜 불필요한 재귀를 방지하고 효율적인 테스팅을 가능하게 했다.

# peer session

## **스스로 확인할 사항**

`expect(result).toBe(expected)` 와 같은 API 형태를 일반적인 테스트 라이브러리에서 흔하게 찾아볼 수 있다. 미션에서 구현한 코드는 `test(message, function)` 의 형태였는데, 라이브러리는 일반적으로 API 형태를 이용해 더 직관적으로 사용할 수 있게 하는 듯 하다.

test 출력의 가장 다른 점은 모든 실행결과가 출력되는 것이 아니라, 우선 **pass/fail 된 코드의 갯수**를 출력하고 **fail 된 코드의 에러메세지만 출력**하는 것이다. **함수 실행에 걸린 시간**이 출력되는 점도 다르다.

## **다같이 확인할 사항**

### **1. 라이브러리에서 제공하는 유용한 기능**

### **mocha**

-   **손쉬운 비동기 테스트 기능**: `done()` 이라는 이름의 콜백을 비동기 코드가 정상적으로 완료됐을 때 호출한다. 기본 timeout 값은 2000ms로, 2초 안에 완료되어 `done()` 이 호출되지 않으면 실패로 간주한다.

거기에서 제공하는 유용한 기능이 있다면, 이를 흉내낼 수 있는 구현 방법을 고민해본다.

유용한 기능은, \***\*함수이거나 동작방식\*\***등 다양하게 선택한다.

-   Hooks: 테스트 케이스마다 반복되는 부분이나 초기화를 위한 메서드가 내장되어 있다.

-   출처: [Mocha로 JavaScript 테스팅 시작하기](https://programmingsummaries.tistory.com/383)

### **2. TDD 방식의 장점과 단점**

-   장점

1. 객체지향적 코드를 작성할 수 있다.

2. 재설계시간이 단축된다. 우선 테스트 코드를 구현해 구현하고자 하는 함수를 짚고 넘어가기 때문이다.

3. 깔끔한 코드가 생기기 때문에 디버깅 및 유지보수 시간이 단축된다.

-   단점

1. 코드 생산성이 떨어진다. 테스트코드를 먼저 구현하고 그에 맞춰 함수를 구현해야하기 때문에, 시간이 많이 걸리고, 개발자들이 TDD를 먼저 익혀야 하기 때문에 비용이 커질 수 있다.

2. 테스트코드 규칙에 맞추기 힘든 경우가 있다.
