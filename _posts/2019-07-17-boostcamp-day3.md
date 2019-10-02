---
layout: post
title: "네이버 부스트캠프 챌린지 Day3"
tags: [boostcamp, js]
---

주제는 JS 언어 익히기. map, reduce, filter, foreach 등 javascript 자체 array method들을 직접 구현해보았다.

## **구현한 것**

### **1. getMatchedType()**

-   배열의 모든 요소를 탐색하며 해야할 일

1. 원하는 타입을 가진 객체 찾기
2. childnode가 있을 시, childnode 로 내려가 탐색 계속하기

-   customReduce로 1번 해결
-   customFilter에서 조건 확인 후 재귀호출로 2번 해결

### **2. customReduce**

-   `Array.prototype.reduce()`: 배열을 돌며 reducer 함수를 실행하고, 최종적으로 하나의 값을 반환하는 함수
-   매번 함수가 실행될 때마다 accumulator의 값을 반환값으로 갱신한 후, for 문이 종료되면 accumulator를 리턴하는 방식으로 구현

### **3. customFilter, customMap, customForEach**

-   `Array.prototype.filter()`: 배열을 돌며 주어진 함수를 실행해 true 값이 반환되는 요소만 모은 배열을 반환하는 함수
-   `Array.prototype.map()`: 배열을 돌며 주어진 함수를 실행한 요소들을 모아 새로운 배열로 반환하는 함수
-   `Array.prototype.forEach()`: 배열을 돌며 주어진 함수를 실행. map과 다른 점은 반환값이 없다는 것
-   그 외 `.some()`과 `.every()`도 구현해 봄
-   함수를 인자로 받은 뒤, custom 함수 내부에서 직접 설정한 인자를 담으면 실행이 된다는 것을 이용해 구현

## **고민**

-   reduce에 쓰이는 reducer 함수는 분리하는 게 더 좋은걸까?
-   getMatchedType()을 구현할 때, reduce와 filter를 써서 두 번 탐색하는 방식으로 했는데, reducer 안에서 한 번에 구현하는 게 더 바람직한지 고민
-   custom 함수를 만들 때, 인자 예외처리가 어려움. 어떻게 접근해야할지 고민
