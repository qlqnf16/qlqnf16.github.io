---
layout: post

title: "디지털 논리회로, 플립플롭"

categories: [computer-science]

date: 2020-01-08
---

회로란 무엇인가...

---

### 개념/용어

- 진리표(Truth Table): 모든 입력에 대한 모든 출력을 표시한 것
- BCD(Binary Coded Digit): 10진수 0-9를 4bit 이진수로 표시한 코드
- LE: Latch Enable
  LT: Lamp Test - 활성화 시 모든 램프에 불이 들어옴
  BI: Blank Input - LT와 반대
- HDL: Hardware Description Language 하드웨어를 소프트웨어적으로 설계하기 위한 언어
- **조합회로** (Combinational Logic): 오로지 입력조건에 의해 출력이 결정되는 회로
- **순차회로** (Sequential Logic): 과거의 출력상태가 현재의 출력에 영향을 끼치는 회로

## 논리연산

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fc72d3f6-a163-49aa-aa12-ffb8265f6399/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fc72d3f6-a163-49aa-aa12-ffb8265f6399/Untitled.png)

- 드모르간 법칙: (AB)' = A' + B' 또는 (A+B)' = A'B'
- 두 논리게이트가 같음을 증명하는 것은 두 진리표가 같음을 이용한다

### Sum of Product

- 진리표를 회로로 설계하는 방법
- 진리표에서 1이 되는 항을 product로 구현
  구현한 항들을 모두 sum해 완성

- Universal Gate: NAND나 NOR만 있으면 어떤 논리게이트도 설계가 가능하다

### 비트계산기 - Half Adder & Full Adder

- Half Adder(반가산기)
  - Sum(합) XOR
  - Carry(자리올림수) AND
- Full Adder(전가산기)
  - 반가산기를 조합해 구현한다

[https://camo.githubusercontent.com/7b592d21090888ccd5edba0413f6a3c649978969/687474703a2f2f7075626c69632e636f646573717561642e6b722f6a6b2f637332332f73746570312d66756c6c61646465722e706e67](https://camo.githubusercontent.com/7b592d21090888ccd5edba0413f6a3c649978969/687474703a2f2f7075626c69632e636f646573717561642e6b722f6a6b2f637332332f73746570312d66756c6c61646465722e706e67)

- Ripple Adder: Full Adder 4개를 연결한 것 (4비트 연산)
  - 처리속도가 느리고 값의 안정성이 떨어지기 때문에 실제로는 사용하지 않는다
    ⇒ 범용 IC (ASIC) 로 제품화되어 있음

### 집적회로 (Integrated Circuit)

특정 기능을 하는 회로를 집적화 시킨 것. 집적 규모에 따라 MSI, LSI 등으로 분류하기도 함.

- **ASIC** (Application Specific IC): 특정 목적을 위해 만든 IC. 소리를 출력하는 ASIC, LED 출력을 제어하는 ASIC 등. IC를 이용해 만들어진 모든 것을 ASIC이라고 할 수 있음
- **SoC** (System on A Chip): 특정 제품을 만들기 위해 필요한 하드웨어를 하나에 칩에 모아놓은 것. CPU가 들어간다.
- **IP** (Intellectual Property): 지적재산권. 여기에선 IC 회로 설계도면을 의미. 반도체 제품이 아니라 설계도면을 지적재산권의 형태로 판매할 수 있다. 스마트폰에 내장된 ARM의 CPU가 대표적 예.

- 7-Segment: 도트 포함 8개의 개별 LED로 숫자 표시기를 만든 것
  - Common-anode type: 0에 점등
  - Common-cathode type: 1에 점등

## 순차회로

### SR Latch

가장 간단하게 구현 가능한 순차회로. R(Reset)에서 Q = 0, S(Set)에서 Q = 1 으로 변하며, R=S=0 일 때는 기존 Q값이 그대로 유지된다. R=S=1 은 모순된 값을 출력하므로 금지된 입력이다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9a00508c-dcb9-40b4-96a8-ea3e89485de8/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9a00508c-dcb9-40b4-96a8-ea3e89485de8/Untitled.png)

- R(Reset): Q를 0으로 바꿈
- S(Set): Q를 1로 바꿈
- R=S=0: 이전 값을 그대로 유지 (Q ⇒ Q)
- R=S=1: 오류. 금지된 입력

### Clocked SR Latch

**SR Latch의 문제점**: 입력신호의 변화속도가 달라 오차가 발생하거나, 노이즈로 인한 금지된 입력이 발생할 수 있음

이 문제점을 보완하기 위한 것. CP(Clock Pulse)가 H 일 때만 S, R의 입력값이 적용되고, L 일 때는 현재 상태가 유지된다.

- 신호는 0, 1 사이에서 변화하는데, 변하는 속도가 다를 수 있다. 또한 중간에 예상치 못한 노이즈가 발생할 수 있다. 대표적으로 0에서 1로 변하는 속도가 1에서 0으로 변하는 속도보다 느리다. 이는 순차회로 처리과정에서 원하지 않던 입력이 발생할 수 있음을 의미한다.
  ⇒ 이를 해결하기 위한 것이 Clocked SR Latch
- 특정 Clock 상태에서만 latch가 발생한다. 클럭 사이의 변화는 무시된다. 클럭이 H또는 L인 동안에만 값을 변경한다
- RS Flip-Flop: latch와 달리 clock에서 edge가 발생할때만 상태 변화. edge란 (H⇒L / L⇒H) 로 클럭의 상태가 변화는 때를 말함.
  - Rising Edge: L ⇒ H / C로 표현
  - Falling Edge: H ⇒ L / C' 로 표현

## FLIP-FLOP

### 1. RS Flip-Flop

Clocked SR Latch 는 H 상태에서만 작동한다. Flip-Flop 은 클럭의 edge에서만 작동한다.

- Rising edge: Low -> High 에서 작동
- Falling edge: High -> Low 에서 작동

### 2. JK Flip-Flop

RS Flip-Flop 에서 사용하지 않는 `R=S=1` 입력을 사용할 수 있도록 변형한 것. 이 경우 기존 Q 값의 반대 (Q') 가 된다 (기존 출력을 뒤집는다).

### 3. D Flip Flop

D 하나만을 입력받는다. D=Q로 작동한다. **PR(Preset)**과 **CL(Clear)**은 각각 클럭에 `비동기적`으로 값을 1, 0 으로 바꾼다. 작동이 끝난 후엔 클럭 발생 전까지 변하지 않는다.

- **D Flip Flop**: JK F/F에서 latch만 남긴 것. Q=D
  - 클럭 Edge에서 F/F 내부에 저장된 D값으로 Q값이 변경됨
  - **PR** (preset): Q = 1 로 변화
    **CL** (clear): Q = 0 으로 변화
    \*\* PR과 CL은 클럭과 무관하게 인가시 바로 작동함. 비동기적으로 작동. 그러나 꺼진 뒤에는 클럭이 발생하기 전까지 원상태로 돌아오지 않음

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/003ce76a-fa55-45bd-a059-10f673234207/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/003ce76a-fa55-45bd-a059-10f673234207/Untitled.png)

(좌) RS Flip-Flop의 심볼 (우) D Flip-Flop의 심볼

### 74xx191

4bit Pre-Settable U/D Counter 범용 IC

- 4개의 Flip-Flop 으로 구성
- PL (Pre Load): D에 설정한 초기값이 설정됨.
- TC (Terminal Count Out): 값이 종단에 이르면 (up에서 `0xF`, down에서 `0`) 1펄스 구간의 1출력(램프 점멸)
- RC (Ripple Clock Output): 값이 종단에 이르면 1/2 펄스 후에 0 출력(램프 점등)
  - 191을 두 개 이상 연결해 8비트 이상의 카운터를 구현할 때 사용
  - 낮은 비트의 RC를 높은 비트의 클럭 입력으로 사용

### 주파수 발생회로

자동으로 고정 주파수의 pulse를 발생시키는 주파수 발생회로가 있다. 발진기라고도 함.

주파수가 높은 경우, **JK F/F를 이용한 분주회로**로 주파수를 낮출 수 있다. 매 F/F 마다 주파수가 1/2로 감소되고, 주기는 2배 길어짐. 매우 간단하게 구현할 수 있다. 주파수를 높이는 건 어렵지만, 낮추는 것, 즉 분주는 쉽다!
