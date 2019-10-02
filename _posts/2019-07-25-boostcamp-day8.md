---
layout: post
title: "부스트캠프 챌린지 Day8"
tags: [boostcamp, js, design pattern]
---

자바스크립트의 대표적인 디자인 패턴 중 하나인 Observer 패턴을 구현해봤다. 평소 React 같은 프레임워크를 사용했는데, 이것도 일종의 Observer pattern으로 구현된 라이브러리인 듯 하다. 프레임워크의 힘으로 편하게 사용했던 걸 직접 구현해보니 또 새로웠다.

## **Observer 패턴**

모듈간의 상호의존성을 최소화하기 위해 이용한다.

이번 미션에서 Observer는 다음과 같이 구현했다. 구독할 대상이 한정적이고 많은 함수를 구현할 필요가 없어서 간단하게 구현했다. `register`로 함수를 observer에 등록해 구독할 수 있다. 구조가 복잡해지면 `this.observers`에 함수를 배열로 계속 `push` 해 `forEach` 문을 이용해 사용하는 것 같다.

```js
class Observer {
    constructor() {
        this.observers = [];
    }

    register(f) {
        this.observers = f;
    }

    notify(data) {
        this.observers(data);
    }
}

module.exports = Observer;
```

이를 이용해 프로젝트에서 적용한 코드는 다음과 같다.

```js
// TodoApp.js
// TodoHtmlView 클래스를 변수에 할당해준 후, todoModel에 등록시켜 구독할 수 있게 했다.
const view = new TodoHtmlView(todoModel);
todoModel.register(view.listen);

// TodoHtmlView.js
// html 파일을 작성하는 updateLog 함수를 구현 후, listen 함수에서 호출되게 했다.
// 이 때 바뀐 값(현재 리스트)를 파라미터로 받아 log.html 파일이 업데이트 될 수 있도록 했다.
class TodoHtmlView {
  ...
  listen = status => {
    this.updateLog(status);
  };
}
```

[참고자료1](http://blog.naver.com/PostView.nhn?blogId=c_ist82&logNo=220795909036&parentCategoryNo=&categoryNo=9&viewDate=&isShowPopularPosts=false&from=postView)

[참고자료2](https://j911.me/2018/10/observer-pattern.html)

## **immutable 상태 관리**

상태를 변화시키지 않고, 복사해서 변경하는 것을 말한다. React 등 최신 프레임워크에서 지향하는 방식으로 불필요한 렌더링 등을 방지해 효율적인 구동을 가능하게 한다.

다음과 같이 기존 데이터에 push 하는게 아니라 […] 구문으로 복사해서 변경하는 방식을 이용했다.

```js
const newData = [
    ...this.data,
    { id: id, title: title, tag: parsedTags, status: "todo" }
];
this.data = newData;
```
