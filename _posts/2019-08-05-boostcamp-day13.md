---
layout: post
title: "부스트캠프 챌린지 Day13"
categories: [boostcamp, computer-science]
---

tokenizer/lexer/parser, 그리고 정규표현식!

### **tokenizer**

- tokenizer 는 **의미있는 단위로 쪼개는 역할**을 한다
- 정규표현식을 이용했다. 처음엔 조건문으로 구현했으나, 정규표현식을 이용해 한 줄 안에 처리가 가능해졌다.

### **lexer**

- lexer는 쪼개진 데이터에 각각 다시 의미를 부여하는 역할을 한다.
- 미션의 요구사항에 따라 **괄호, 숫자, 문자, NULL**로 분리해주었다.

        [
          { type: "LBracket", value: "[" },
          { type: "number", value: 1 },
          ...{ type: "string", value: "hello, world" },
          { type: "NULL", value: null },
          { type: "RBracket", value: "]" }
        ];

### **parser**

- parser는 의미를 부여한 데이터를 object로 구조화하는 역할을 한다.
- child 에서 child 로 노드가 꼬리를 물고 이어지는 형태를 구조화하는 게 어려웠다.
  - 각각의 노드를 저장하는 배열 `nodes` 와 현재 노드의 인덱스를 저장하는 변수 `nodeIdx` 를 설정해 구현했다.
  - 괄호가 열리면 nodes 에 push 하고, nodeIdx에 1을 더한다. 괄호가 닫히면 nodeIdx에서 1을 빼고 nodes를 pop한다.
  - 이 방법은 노드들을 모두 넣어둬야 해서 조금 비효율적인 것 같은데, 더 나은 방법을 고민해봐야겠다.

참고: [https://pa-pico.tistory.com/91](https://pa-pico.tistory.com/91)

## **정규표현식**

문자열의 검색과 치환을 위한 용도로 쓰인다. 일반적인 조건문으로는 다소 복잡할 수도 있는 조건을 정규표현식을 이용하면 매우 간단하게 표현 할 수 있다.

**day13** 미션을 위해 구현한 정규표현식은 다음과 같다.

```js
const regex = /**\[**|**\]**|\w+|'(.*?)'/g;
```

`\[|\]` 를 이용해 괄호들을 걸러내고, `\w+` 로 간단한 숫자나 문자열을 구분할 수 있다. 또한 quotation mark 안에 들어있는 문자열은 그 안에선 어떤 문자가 오든 상관 없기 때문에 **`'(.*?)'` 로 표현해서 처리**해줬다.

[ 정규표현식 메소드 모음 (출처: MDN 정규표현식)](https://www.notion.so/1168f83bee4745a2afeda0b96135a35c)

### **참고)**

**[정규표현식 테스트 사이트](https://regexr.com/)**: 바로 테스트해 볼 수 있고, 사용한 정규표현식에 대한 설명도 나와 있어 편리함

[정규표현식 추천](http://txt2re.com/index-javascript.php3): 문자열을 입력하면 사용할 만한 정규표현식을 추천해준다

## **테스트 코드**

이번 미션에서는 **Chai.js** 를 이용해 봤다가, 내장 `assert` 모듈로도 충분히 테스팅이 가능할 것 같아서 assert를 이용했다. `assert.equal` 은 오브젝트 비교가 불가능하므로 **`assert.deepEqaul`** 을 사용해 테스팅했다.
