---
layout: post
title: "JAVA Thread"
categories: [dev, java]
---

스레드(Thread)에 대해 알아보자

---

Thread란, 프로세스를 작은 단위로 나눈 블록으로, 프로세스가 미리 할당받은 메모리를 사용한다.

프로그래밍 언어는 각각 싱글스레드이거나 멀티스레드인데, 자바는 대표적인 멀티스레드 언어이다. **멀티스레드는 한 번에 두 개 이상의 작업을 동시에 처리할 수 있으므로 싱글스레드보다 효율적**이다. 하지만 자원이 충돌하는 등의 버그가 생길 확률이 높아 코딩이 어려워진다는 단점이 있다.

자바의 스레드는 다음과 같이 동작한다.

> 1. JVM에 의해 하나의 프로세스가 생성되고, main() 안의 실행문들이 하나의 스레드이다.
> 2. main() 외의 또 다른 스레드를 만들기 위해서는 **1) Thread 클래스를 상속**하거나, **2) Runnable 인터페이스**를 구현한다. 다중상속이 가능하므로 2번 방법을 더 자주 쓴다.
> 3. 멀티 스레드 작업 시에는 각 스레드끼리 정보를 주고받을 수 있다.

## Thread 구현

### 1. Thread class

Thread를 상속받아 스레드를 구현하기 위해서는, `.run()` 메소드를 오버라이딩 해서 사용해야 한다. Thread로 생성된 객체는 `.start()` 로 동작하는데, `.start()` 는 자동으로 `.run()`을 호출한다.

```java
class ThreadEx extends Thread {
  public ThreadEx (String name) {
    super(name);
  }

  public void run() {
    for (int i = 1; i <= 5; i++) {
      for (int j = 1; j <= 1000000; j++) {
        System.out.println(getName() + " -> " + i);
      }
    }
  }
}

public class ThreadsTest {
	public static void main(String[] args) {
    ThreadEx t1 = new ThreadEx("thread 1");
    ThreadEx t2 = new ThreadEx("thread 2");
    ThreadEx t3 = new ThreadEx("thread 3");

    t1.start();
    t2.start();
	}
}
```

### 2. Runnable 인터페이스

**Runnable 을 implement 하는 클래스를 정의**해서 사용한다. Thread 클래스와 마찬가지로, `.run()` 메소드를 정의하고 객체를 생성해 `.start()` 로 호출한다.

```java
class Example implements Runnable {
  public void run() {
    for (int i = 1; i <= 5; i++) {
      for (int j = 1; j <= 1000000; j++) {
        System.out.println(getName() + " -> " + i);
      }
    }
  }
}

public class ThreadsTest {
	public static void main(String[] args) {
    Example r1 = new Example();
    Example r2 = new Example();

    Thread t1 = new Thread(r1);
    Thread t2 = new Thread(r2);
    t1.start();
    t2.start();
	}
}
```

## Thread 동기화

멀티스레드 프로그래밍에서는 여러 개의 스레드가 동시에 하나의 자원에 접근할 수 있다. 이로 인해 발생할 수 있는 오류를 차단하기 위해 `synchronized` 키워드를 이용해 스레드를 동기화 시킬 수 있다.
