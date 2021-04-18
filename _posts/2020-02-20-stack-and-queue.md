---
layout: post
title: "스택과 큐, 원형 큐"
tags: [data-structure]
---

c를 이용해 스택과 큐, 원형큐를 다양한 방법으로 구현해보자

## 스택 (Stack)

후입선출의 구조(**LIFO**, Last In First Out). 배열과 힙을 기반으로 구현할 수 있다. 배열 기반의 구조를 더 자주 사용한다. **스택 포인터의 시작을 MAX값**으로 잡아 1씩 줄여 스택 포인터가 0이 되면 스택이 꽉 찼다고 보는 것이 **descending**, 0부터 올라가는 것이 ascending 방식이다. descending 방식으로 구현하는 것이 일반적이다.

**배열 기반 스택 구현**

```c
#define MAX_STACK  10
#define EMPTY_STACK MAX_STACK
#define FULL_STACK  0

int Stack[MAX_STACK];
int Sptr = EMPTY_STACK;
int Push_Stack(int data){
 if (Sptr == FULL_STACK) return -1;
 Stack[--Sptr] = data;
 return 1;
}

int Pop_Stack(int *p) {
 if (Sptr == EMPTY_STACK) return -1;
 *p = Stack[Sptr++];
 return 1;
}
```

**힙 기반 스택 구현**

```c
#include
#define FULL_STACK  0

typedef struct stk {
 int num;
 struct stk* next;
} STACK;
STACK Sptr;

int Push_Stack(STACK * data){
  STACK * p;
  p = calloc(1, sizeof(STACK));
  if (!p) return -1;
  *p = *data;
  p->next = Sptr;
  Sptr = p;
  return 1;
}

int Pop_Stack(STACK* p) {
  if (!Sptr) return -1;
  *p = *Sptr;
  Sptr = Sptr->next;

  return 1;
}
```

## 큐 (Queue)

선입선출의 구조(**FIFO**, First In First Out). **읽기를 위한 포인터**와 **쓰기를 위한 포인터** 두 개를 이용해 기능을 구현할 수 있다. 데이터를 삽입하는 동작을 enqueue, 가져오는 동작을 dequeue라고 한다. 알고리즘에서 BFS구현에 이용된다. **Wrptr이 MAX**에 도달했을 때 queue가 꽉 찼다고 보고, **Wrptr과 Rdptr이 같을 때** queue가 비었다고 본다.

Queue에서 삽입과 삭제를 반복하다보면 <u>앞 공간이 빈 상태로 낭비</u>되어 본래 큐가 담을 수 있는 크기보다 적은 데이터를 담게 된다. 이를 방지하기 위해 선형 큐에서는 **Wrptr이 MAX에 도달하고 Rdptr이 0보다 크면**, Rdptr이 0이 될때까지 데이터를 앞으로 끌어오는 방식을 사용한다.

**배열기반 선형 큐 구현**

```c
#define MAX_QUEUE    10
#define QUEUE_EMPTY  0
#define QUEUE_FULL   MAX_QUEUE

int Queue[MAX_QUEUE];
int Wrptr;
int Rdptr;

int Enqueue(int data) {
  if (Wrptr == QUEUE_FULL && !Rdptr) return -1;
  if (Wrptr == QUEUE_FULL) {
    for (int i = 0; i < Wrptr-Rdptr; i++) {
      Queue[i] = Queue[i+Rdptr];
    }
    Wrptr = Wrptr-Rdptr;
    Rdptr = 0;
  }
  Queue[Wrptr++] = data;
  return 1;
}

int Dequeue(int *p) {
  if (Rdptr == Wrptr) return -1;
  *p = Queue[Rdptr++];
  return 1;
}
```

### 원형 큐 (Circular Queue)

Wrptr이 끝에 도달할 때마다 데이터를 옮기는 것은 구현도 복잡하고 시간도 오래 걸린다. 가령 큐의 크기가 100만개 쯤 된다면 enqueue가 일어날때마다 99만개 이상의 데이터를 옮겨야 할 수도 있는 것이다. 따라서 선형 큐는 잘 쓰이지 않고, 이를 보완한 **원형큐**를 사용한다. 원형 큐는 **Queue의 시작과 끝을 이어붙여 원형으로** 만든 형태이다.

원형큐에서 포인터는 끝에 도달하면 다시 시작점으로 돌아간다. 따라서 **0~MAX-1**을 순환한다. 다만 값을 순환시키다보면 Wrptr과 Rdptr 중 어느것이 앞서 있는지 판단할 방법이 없게 된다. 이를 해결하기 위해 항상 **큐의 맨 끝자리는 비워둔다**. 즉 원형큐에서 `Wrptr+1 == Rdptr` 이면 큐가 꽉 찼다고 본다.

**배열기반 원형 큐 구현**

```c
#define MAX_QUEUE    10
#define QUEUE_EMPTY  0
#define QUEUE_FULL   MAX_QUEUE

int Queue[MAX_QUEUE];
int Wrptr;
int Rdptr;

int Enqueue(int data) {
  if ((Wrptr+1) % MAX_QUEUE == Rdptr) return -1;
  Queue[Wrptr++] = data;
  Wrptr %= MAX_QUEUE;
  return 1;
}

int Dequeue(int* p) {
  if (Wrptr == Rdptr) return -1;
  *p = Queue[Rdptr ++];
  Rdptr %= MAX_QUEUE;
  return 1;
}
```
