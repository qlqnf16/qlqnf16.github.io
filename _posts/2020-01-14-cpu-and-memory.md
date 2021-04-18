---
layout: post

title: "마이크로 프로세서"

tags: [computer-science]

date: 2020-01-14
---

CPU와 메모리의 구조, 작동방식에 대해 알아보자

---

## CPU

### ALU (Arithmetic Logic Unit)

CPU에서 입력된 값으로 연산을 수행하는 장치. 조합회로로 설계됨

- Instruction (명령어)
  명령어는 이진수 값들로 구성돼 메모리에 저장된다. CPU는 fetch 를 통해 명령어를 메모리에서 받아온다. 명령어는 Opcode 와 Operand로 구성된다. - **Opcode**: 연산 동작을 수행할 명령 코드. 다시 명령군과 명령으로 구분된다. 명령군은 이동/비교/분기 등 큰 명령군을, 명령은 add, sub 등 명령군 내 세부적인 명령을 표시한다. - **Operand**: 연산에 사용하는 자료. 연산할 대상. 값이 저장된다.
- Status: 연산의 결과를 나타내는 신호. Z, N, C 등의 비트를 가짐

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/79a9e95c-ef5c-4635-8617-75f85a79228b/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/79a9e95c-ef5c-4635-8617-75f85a79228b/Untitled.png)

ALU의 구조

### Register (Registered Memory)

연산에 필요한 데이터나 연산결과를 임시로 기록, 저장하는 장치. 레지스터는 각 목적에 따라 수많은 종류의 레지터의 집단으로 구성되는데, 대표적으로 PC가 있다.

- PC(program counter): 0부터 값을 상승시킨다. 메모리에서 명령어를 받아올 때 사용된다.

### Control Unit (제어회로)

ALU, Register, 외부 메모리 등과 연결되어 모든 흐름을 제어함. CPU 작동의 핵심 역할.

## 메모리

프로그램과 데이터를 저장하는 공간. 크게 _ROM 과 RAM으로 구분된다_. ROM과 RAM을 구분짓는 가장 큰 기준은 다음과 같다.

[RAM vs ROM](https://www.notion.so/1f34698fb85245a7aeb980f642079039)

### ROM

일반적으로 코드를 저장하는 용도로 사용된다. 전원을 껐다 켜도 데이터가 유지된다는 특징이 있다. 하드웨어 기술이 발전함에 따라 ROM도 발전을 거듭해 왔다.

- Mask ROM: Fab(IC 제조회사)에서 코드를 write해서 생산한 것. 싼 가격에 대량생산.
- OTP ROM (One Time Programmable ROM): 코드를 딱 한 번 writing 할 수 있는 ROM.
- EPROM (Erasable PROM): 자외선을 이용해 코드를 지울 수 있음. Rewritable ROM.
- **EEPROM** (Electrically Erasable PROM): 회로에서 전기신호로 코드를 지울 수 있음. => 펌웨어 업그레이드가 가능해짐
- **NOR Flash**: 플래쉬 메모리라고도 함. EEPROM을 개선시킨 것으로, 요즘 가장 상용화됨.
- cf) NAND Flash: 대용량. 코드메모리로는 사용 불가. 하드디스크로 상용화.

### RAM

데이터를 저장하는 용도로 사용된다. 전원을 껐다 키면 저장된 것이 모두 사라지고 초기화된다.

- SRAM (Static RAM): 전기가 공급되는 동안 저장된 데이터가 내내 유지
- DRAM (Dynamic RAM): 전기가 공급되는 동안에도 주기적으로 `Refresh (1로 전기신호를 갱신)`해야 데이터가 유지. SRAM에 비해 속도, 용량 등 전반적 성능 향상
- SDRAM (Synchronus DRAM): CPU의 클럭과 동기화시켜 고속으로 입출력을 수행함으로써 DRAM에 비해 빠른 처리속도
- **DDRx SDRAM** (Double Data Rate SDRAM): Rising, Falling edge 모두에서 입출력을 수행해 SDRAM보다 빠른 고속 RAM

### 메모리 분석

반도체 메모리: 8/16/32bit 메모리 셀의 집합 소자

- 일반 메모리의 신호
  - A[n:0]: 셀 선택을 위한 address bus
  - D[m:0]: 선택된 셀에 값을 쓰거나 읽는 data bus
  - CS(Chip Select): 활성화 신호. CE(Chip Enable) 라고도 함
  - WE: Write Enable. 외부에서 메모리에 쓰기 위한 신호
  - OE: Output Enable. 메모리의 값을 읽어가기 위한 신호
- 메모리의 주소 개수: 2^a 개 (a = address bus 의 비트 수)
- 메모리의 용량: `(메모리의 주소 개수) * (data bus의 비트 수)`
- 메모리의 주소 번지: 0부터 `(용량 / B)-1`까지

## CPU의 동작방식

CPU는 clock에 동기화되어 동작. 하나의 Rising Edge 당 하나의 동작을 수행한다.

### **CPU 동작의 3 phase**

- Fetch: PC에 따라 주소에 맞는 메모리에서 명령어를 가져오는 것 (메모리 읽기)
- Decode: 명령어의 Opcode와 Operand에 따라 연산을 수행하기 위해 ALU를 준비하는 단계
- Execute: 디코딩된 명령에 따른 일을 수행하는단계 (ALU실행)
- 3phase가 완료돼야 명령 하나가 완료됨. 즉, 명령 하나를 완료하는데 3clock이 필요하다
  => `3CPI (Cycles Per Instruction)` 이라고 부른다.
  예를 들어 300MHz Clock이 입력되면, 100MIPS로 동작한다.

### 명령어 파이프라인 Pipeline

- 3phase 를 병렬적으로 실행하는 것
  한 명령어가 fetch 된 후 decode 될 때, 다음 명령어를 fetch 하는 방식
- **장점**: 실행속도가 매우 빨라짐. 1CPI 가능
- **단점**: 문제가 생기면 모두 비우고 다시 채워야하는 등, 해결이 어려움

### CPU의 내부 구조

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9d729956-c057-477e-9b96-5752dac22a0b/Screen_Shot_2020-01-08_at_8.21.48_PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9d729956-c057-477e-9b96-5752dac22a0b/Screen_Shot_2020-01-08_at_8.21.48_PM.png)

### CPU와 메모리

**메모리 버스**

- address bus: 메모리의 주소값을 전송
- data bus: 데이터를 읽어오거나 쓰는 데 사용
- control bus: 각종 CPU 동작에 필요한 입력 제어

**메모리 읽기와 쓰기 Timing**

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0dde6bab-ec8b-4fb3-a3ee-d130f5669ca8/KakaoTalk_Photo_2020-01-08-20-22-28.jpeg](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0dde6bab-ec8b-4fb3-a3ee-d130f5669ca8/KakaoTalk_Photo_2020-01-08-20-22-28.jpeg)

**Von Neumann Architecture**

Stored Program 방식. 프로그램을 메모리에 저장해놓고 F-D-E 사이클을 반복하며 순서대로 실행하는 것

---

## CPU의 분류

- CISC (Complex Instruction Set Computer)
  - Intel x86 으로 대표되는 고전적 프로세서
  - 많은 명령어가 존재함 ⇒ 성능이 높고 효율적
  - 전력소비가 큼 ⇒ PC, 자동차, 가전 등에 사용
- RISC (Reduced Instruction Set Computer)
  - ARM 이 제안한 개념
  - CISC에 비해 기본적 동작을 수행하는 최소한의 명령어만을 가짐
  - 컨트롤 유닛이 간단해짐 ⇒ 발열/가격/전력 소비량 등이 줄어듦
  - 성능은 CISC에 비해 떨어짐 ⇒ 스마트폰, IoT장치 등에 사용

입출력장치 / 주변장치 (peripherals): CPU 단독으로는 할 수 없는 일들을 가능하게 하는 추가적인 보조장치

ex. LED를 켜기위해선 LED를 제어하는 입출력장치가 필요한 것

### CPU Bus

1. Address Bus (단방향)
2. Data Bus (양방향)
3. Control Bus

- **Memory Mapped I/O**
  - 입출력장치에도 메모리처럼 주소를 부여하는 것
  - CPU입장에서는 입출력장치도 메모리처럼 인식됨
  - 명령어를 줄여 Control Unit을 단순화하기 위해 사용 ⇒ RISC

### Coprocessor

: 프로세서 (CPU)의 연산을 돕는 장치. CPU의 성능 향상에 기여함

- FPU (Floating Point Unit): 복잡한 실수연산을 담당하는 장치
- GPU (Graphic Processor Unit)

### Cache 메모리

CPU와 메모리 사이에 장치해 처리속도를 높이는 저장소. 용량이 매우 작음.

- CPU - Register - Cache(s) - Memory - NAND(Hard Disc)
- 접근하려는 메모리가 Cache에 있을 때를 `hit`, 없을 때 `miss` 라고 한다 ⇒ 히트율이 높을수록 효율적인 cache
- 캐시도 cpu와 가까운 순서에 따라 여러 레벨로 나뉠 수 있다
  ⇒ 히트율이 높은 메모리를 cpu와 가까이 위치시켜 효율성을 높이기 위해

- big.LITTLE 구성: 속도나 성능이 중요한 프로세스는 big으로, 전력이 중요한 프로세스는 LITTLE로 처리하는 것
  **상황에 따라 다른 프로세서**를 사용함으로써 속도와 배터리 수명 모두를 해결할 수 있음
