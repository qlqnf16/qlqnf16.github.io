---
layout: post
title: "React에서 API KEY 숨기기"
category: js
---

오랜만에 프로젝트 하려니 API KEY를 어디에 둬야 하는지 모르겠어서(..) 앞으로 까먹지 않기 위해 정리

### 루트 폴더에 .env 파일 생성 후 .gitignore 에 .env 추가

```js
// .env

REACT_APP_NAME = "key";
```

위와 같은 형태로 작성한다. 모든 이름은 REACT*APP* 로 시작해야함 주의!

### API KEY 불러와서 사용하기

```js
const API_KEY = process.env.REACT_APP_NAME;
```
