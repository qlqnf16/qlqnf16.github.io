---
layout: post
title: "부스트캠프 챌린지 Day12"
tags: [boostcamp, js]
---

# **CANVAS API**

## **Get Started**

    npm install canvas

```js
const { createCanvas } = require("canvas");
const canvas = createCanvas(600, 400);
const context = canvas.getContext("2d");
```

npm install로 canvas 모듈을 설치하고 `createCanvas()` 명령으로 초기 세팅을 할 수 있다. `getContext()` 를 이용해 2d 좌표평면 환경을 설정한다.

## **스타일링**

### **기본 스타일링**

```js
context.fillStyle = "#000000";
context.strokeStyle = "rgb(0,0,0)";
context.lineWitdh = 3; // stroke 굵기 설정
context.setLineDash([5, 3]); // stroke dash 설정
context.fill(); // 도형 채우기
context.stroke(); // 선 그리기
```

### **`createLinearGradient(x1, y1, x2, y2)`**

```js
const grd = context.createLinearGradient(
    center - radius,
    center + radius,
    center - radius,
    center + radius * 3
);
grd.addColorStop(0, "#fef362");
grd.addColorStop(1, "#ee2a0f");
```

그라디언트의 시작지점(x1, y1)과 종료지점(x2, y2)을 설정해 그라디언트의 방향을 정해줄 수 있다. `addColorStop`은 0부터 1 사이에서 원하는 만큼 추가해 줄 수 있다.

## **도형 그리기**

### **직선 그리기: `lineTo(x, y)`**

```js
context.moveTo(center - radius * 0.4, center + radius * 3);
context.lineTo(center - radius, 400);
context.lineWidth = 3;
context.stroke();
```

좌표를 옮기는 `moveTo()` 명령어를 이용해 시작점을 설정하고, 해당 좌표까지 선을 긋는 `lineTo` 명령어를 이용해 직선을 그릴 수 있다.

### **사각형 그리기**

```js
context.fillStyle = "#60d936";
context.fillRect(0, 0, 600, 400);
```

`fillRect(x1, y1, x2, y2)` 명령어는 지정된 좌표에 직사각형을 만들고 칠한다. 이 때 색상은 `fillStyle`에 지정되어 있는 색상으로 적용된다.

### **원 그리기: `.arc(x, y, radius, startAngle, endAngle, antiClockwise)`**

```js
context.arc(x, y, radius, 0, Math.PI * 2);
context.fillStyle = "white";
context.fill();
```

-   원의 중심 좌표: (x, y)
-   반지름: radius
-   startAngle, endAngle: 원을 그리기 시작하는 각도와 끝나는 각도. **참고:** `arc` 함수에서 각도는 각이 아닌 라디안 값을 사용한다. 각도를 라디안으로 바꾸려면 다음의 코드를 사용할 수 있다: `radians = (Math.PI/180)*degrees`
-   기본 설정은 시계방향으로 원이 그려진다. 다섯번째 인자로 `true`를 넘겨주면 반시계방향으로 그릴 수 있다.

### **+) 부채꼴 그리기**

```js
context.arc(
    center,
    center,
    radius,
    (Math.PI / 180) * -15,
    (Math.PI / 180) * 30
);
context.lineTo(center, center);
context.fill();
```

먼저 `arc`의 시작과 끝 각도를 설정해 호를 그릴 수 있다. 그 후 `lineTo(x, y)`를 이용해 원의 중심 좌표를 인자로 넘겨주면 부채꼴을 얻을 수 있다.

### **글씨 쓰기**

```js
context.font = "30px sanserif";
context.fillText("우와아아", -30, 0);
```

`.font`를 사용해 폰트 크기와 폰트 종류를 지정해 줄 수 있다. 둘 중 하나라도 입력되지 않으면 적용되지 않으므로 둘 다 꼭 입력해 줘야 한다. `fillText()`는 `글의 내용`과 글쓰기 시작 좌표 (`x`, `y`)를 받는다.

## **도형 회전 및 이동**

### **`translate(x, y)`, `rotate(angle)`**

중점을 설정된 좌표로 이동한다. `translate` 명령어로 설정된 좌표가 기본으로 인식된다. 이후 `rotate` 명령어는 설정된 중점을 기준으로 캔버스를 회전한다. 마찬가지로 각도는 각이 아니라 radian 값을 받으므로 `( Math.PI / 180 ) * angle` 로 사용할 수 있다.

### **`save()`와 `restore()`**

여러번 중점을 옮기고 rotate 명령을 실행할 경우 필수적으로 사용해야 하는 명령어들이다. `save()` 는 현재의 설정을 저장하고 `restore()` 는 저장된 설정으로 돌아간다.

```js
context.save();
context.translate(390, 150);
context.rotate((Math.PI / 180) * -30);
context.moveTo(startX, startY);
context.lineTo(startX + size * 3, startY);
context.fill();
context.restore();
```

따라서 위와 같이 translate 명령 전에 save 로 기존 설정을 저장해놓고, 도형 그리기가 끝난 후 restore를 해 주면 초기화가 되어 각각의 도형을 독립적으로 그려줄 수 있다.

## **이미지 그리기: `drawImgae(img, x, y, width, height)`**

```js
const { createCanvas, Image } = require("canvas");

const img = new Image();
img.onload = () => {
    let portrait = true;
    if (img.height < img.width) portrait = false;

    const width = portrait
        ? img.width * ((radius * 2) / img.height)
        : radius * 2;
    const height = portrait
        ? radius * 2
        : img.height * ((radius * 2) / img.width);

    context.drawImage(img, 350, center + radius, width, height);
};
img.onerror = err => {
    throw err;
};
img.src = "./hockney2.jpeg";
```

로컬에 있는 이미지는 위와 같은 방법으로 사용할 수 있다. 첫번째 인자로는 `new Image()`로 생성한 인스턴스를 받고, 그리기 시작 좌표(`x`,`y`)와 `가로` `세로` 길이를 받는다. 이를 이용해서 스케일도 지정해 줄 수 있다.

## **PNG/JPEG로 저장하기**

```js
const fs = require("fs");

const out = fs.createWriteStream("./test.png");
const stream = canvas.createPNGStream();
stream.pipe(out);
out.on("finish", () => console.log("The PNG file was created."));
```

canvas 모듈 내장 메서드 `createPNGStream()` 을 이용해 지금까지 canvas 에 그린 이미지를 실제 .png 파일로 저장할 수 있다.
