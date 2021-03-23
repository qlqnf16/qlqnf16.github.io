---
layout: post
title: "프로세스와 스레드"
category: [OS]
---

프로세스와 스레드의 개념에 대해 알아보자

---

# 프로세스

프로세스는 **실행중인 프로그램**으로, 커널에 등록돼 운영체제의 관리 하에 들어갔으며, 아직 그 수행이 종료되지 않은 상태를 의미한다. <u>CPU가 할당되는 단위</u>를 의미하기도 한다.

작업은 **프로그램과 이에 필요한 데이터**를 의미한다. 즉, 사용자가 컴퓨터에서 실행시키기 위해 작성한 프로그램과, 프로그램의 실행에 필요한 입력 데이터를 묶어서 칭하는 말이다.

프로세스는 여러 상태(생성, 준비, 실행, 대기, 종료) 중 한 상태에 있으며, 상태를 전이하며 수행을 계속한다.

프로세스는 프로그램을 실행하기 위해 커널에게 각종 **자원**을 요청할 수 있다. 자원이란, 기억장치, 프로세서, 디스크 등의 각종 하드웨어 장치나 메시지, 파일 등의 소프트웨어 자원을 통칭한다.

### 프로세스의 종류

- 운영체제 프로세스: 프로세스들의 실행순서를 제어하거나, 사용한 프로세스가 다른 영역을 침범하지 못하도록 예방하는 등 시스템 감시 기능을 담당한다. 프로세스 생성, 입출력 프로세스 생성 등 시스템 운영에 필요한 코드를 수행한다. 커널 프로세스, 시스템 프로세스라고도 한다.
- 사용자 프로세스
- 병행 프로세스

### PCB(Process Control Block)

운영체제가 프로세스를 제어하는 데 필요한 단위.

- 프로세스 식별자: 프로세스를 찾을 때나 프로세스 통신에 이용된다. 이 프로세스를 생성한 프로세스(Parent Identifier), 이 프로세스가 생성한 프로세스(Child Identifier)도 함께 가질 수 있다.

- 프로세스 스케줄링 정보: 우선순위 기반 스케줄링을 할 때 필요한 정보. 프로세스의 우선순위, 스케쥴링 큐 포인터, 다른 스케쥴 매개 변수를 가진다.

- 프로세스 상태 정보

- 프로세스 제어를 위한 정보: 프로세스가 할당받은 자원에 대한 정보를 제어하는 영역.

  - 자료구조 / 프로세스 간 통신 정보 (Flag, message, signal 등) / 권한 정보 / 주기억장치 관리 정보

### 프로세스의 상태

프로세스의 상태는 프로세스가 현재 소유하고 있는 자원들의 종류와 요청중인 자원들의 종류에 따라 구분된다.

- 생성New: 사용자가 요청한 작업이 커널에 등록돼 프로세스가 생성된 상태. 이 때 기억장치에 충분한 공간이 있다면 프로세스를 준비상태로`(dispatch)`, 공간이 없다면 지연준비 상태로 전이시킨다.
- Active: 프로세스가 기억장치를 할당받은 상태
  - 실행Running: **CPU를 할당받아 현재 수행되고 있는 상태**.
    할당받은 시간 할당량이 끝나거나`(timeout)`, 현재 프로세스보다 더 높은 우선순위의 프로세스가 들어오면 프로세서를 반납하기도 한다. 이를 `선점(preemption)`됐다고 표현하며, 선점된 프로세스는 다시 준비상태로 돌아간다.
  - 준비Ready: CPU의 할당만을 기다리고 있는 상태
  - 대기Block: 임의의 **자원을 요청**하고, 이를 할당받을 수 없어 **기다리고 있는** 상태. 예를 들어 입출력을 요구하고, 입출력이 종료될 때까지 기다리고 있는 상태를 의미한다. 요청한 자원을 할당받으면 프로세스는 다시 준비상태로 전이되고, 이를 `wakeup` 이라 한다.
- 종료Exit: 더 이상의 CPU를 요구하지 않고 종료된 상태. 커널 공간 내 프로세스 관리 정보만 남아있는 상태

- Suspended: 프로세스가 기억장치를 할당받지 못한 상태

  프로세스가 지연되는 경우는 **1)** 운영체제가 주기억 공간을 확보하려 하거나 **2)** 프로세스에 문제가 생기거나 정상적인 시행이 현재 불가능 할 때, 또는 **3)** 프로세스의 수행 주기를 맞춰야 할 때가 있다. 지연된 프로세스는 주기억장치가 아닌, **보조기억장치**에 적재된다.

  - 지연 준비 Suspended Ready
  - 지연 대기 Suspended Blocked

#### Context Switching

중단된 프로세스의 현재 상황(정보)를 시스템에 보관하고, 새로 수행될 프로세스를 시스템에 적재시키는 것

# 스레드

프로세스를 효율적으로 조작하는 것은 운영체제의 성능에 큰 영향을 미친다. 프로세스의 조작에는 **프로세스의 무게**가 중요하게 작용하는데, 무게란 프로세스가 사용하는 자원의 논리적 총량을 말한다. 따라서 이 무게를 줄인, **경량 프로세스**라는 개념이 프로세스의 고속 조작을 위해 도입되었고, 이것이 곧 **Context**, **<u>스레드</u>**를 의미한다.

스레드란 **프로세스보다 더 작은 CPU의 실행 단위**로, 프로세스의 상대적 무게를 경감시켜 효율을 올리는 것을 목적으로 한다. 실행의 기본 단위를 스레드로 지정함으로써, **멀티 스레딩**을 통한 동시, 병렬 처리 등이 가능하다.

하나의 프로세스를 구성하는 여러 스레드는, 각각 <u>독립적인 제어흐름을 갖지만</u>, 주소공간 및 <u>시스템 자원은 한 프로세스 내에서 공유</u>하며, 프로그램 카운터, 레지터, 스택 공간은 각 스레드가 개별로 갖게 된다.

### 다중 스레드 Multi threading

여러개의 스레드를 이용해 하나의 프로그램을 동시에 수행하는 것. 하나의 프로세스가 다수의 스레드를 가지지만, **각 스레드는 자원을 공유**하므로 자원 관리의 효율성을 높일 수 있다.

- 하드웨어의 병렬성
- 응용 프로그램의 처리율 향상
- 응용 프로그램의 응답성 증가
- 프로세스 간 통신 향상
- 시스템 자원들의 이용성 개선

# 프로세스 스케쥴링

시스템 내에 여러개 프로세스가 동시에 존재하지만, 하나의 프로세서를 사용할 수 있는 프로세스는 단 하나뿐이므로, 여러 프로세스의 수행 순서를 적절히 정해줘야 한다. 적절한 스케줄링을 통해 사용자는 자신의 프로세스가 항상 실행되고 있다는 느낌을 받을 수 있다.

스케줄링이란 특정 자원을 요청 중인 대상들 중 누구에게 먼저 그 자원을 할당할 지 결정하는 일을 말하며, 프로세스 스케줄링이란 **프로세서를 할당할 순서를 정하는 것**을 의미한다.

### 목적

- CPU 효율을 최대화 하는 것: 주어진 시간 내에 이뤄지는 처리량이 극대화되는 것

- 모든 프로세스에 대한 공평성 유지

- 중요 프로세스에 대한 우선 처리

- 처리 완료 시간을 예측 가능하게 하는 것

- 대화식 프로세스에서 응답시간을 최소화하는 것

- 작업의 대기시간을 최소화하는 것

### 스케줄링에서 고려해야 할 사항

- 프로세스 속성: 입출력 위주인지, 연산 위주인지
- 시스템 속성: 일괄 처리 시스템은 반환 시간을, 대화형 시슽메은 응답시간을 중요시 한다
- 신속한 응답시간의 중요성
- 프로세스의 실행 시간
- 프로세스의 활용 목적
- Page Fault 의 발생 빈도
- 프로세스의 우선순위

### 스케줄링 기법

- 선점 기법

  실행중인 프로세스보다 더 높은 우선순위를 가진 프로세스에게 CPU를 양보하는 스케줄링. Context Switching을 위한 오버헤드가 커진다는 단점이 있다.

  - Round Robin

    사전에 정해진 시간 할당량을 가지고, **프로세스의 수행 시간이 시간 할당량을 넘으면 즉시 다음 프로세스에게 CPU를 넘기고 대기열의 맨 뒤로 가는 방법**. 시간 할당량이 너무 길면 FCFS와 다름 없어지고, 너무 짧으면 Context Switching에 걸리는 시간이 더 많이 걸리는 문제가 생길 수 있다. 따라서 시간할당량을 적절히 설정하는 것이 중요하다.

  - SRT (Shortest Remaining Time)

    프로세스 **대기열에 더 새롭고, CPU 처리시간이 더 짧은 작업**이 들어올 때 그 프로세스가 프로세스를 선점하는 방법. 프로세스의 사용 시간을 알아야 하기 때문에 배치 시스템에 적합하다.

- 비선점 기법

  한 번 CPU를 할당받은 프로세스는, 스스로 CPU를 반납하기 전까지 CPU를 계속 점유하는 스케줄링. 프로세스의 공정성을 확보하고, 응답시간이 예측 가능하다는 장점이 있는 반면, CPU 사용 시간이 긴 프로세스에 의해 다른 프로세스들의 대기 시간이 길어질 수 있다는 단점이 있다.

  - FCFS (First Come First Served) = FIFO

    **대기열에 들어온 순서대로** 프로세스를 수행하는 방법. **배치시스템**에 사용하기 좋다.

  - SJF (Shortest Job First)

    **CPU 사용 시간이 짧은 프로세스부터 수행**하는 방법. 평균 대기시간이 최소가 되는 방법이지만, CPU 사용시간이 긴 프로세스는 기아상태(starvation)에 빠질 수 있다는 단점이 있다. 배치시스템에 적합하다.

  - 우선순위 알고리즘

    프로세스의 우선순위에 따라 **우선순위가 높은 것부터 수행**하는 방법. 메모리 요구량이 적을 수록, 주변장치를 적게 요구할 수록, CPU 사용시간이 짧을 수록 우선순위를 높게 책정한다.