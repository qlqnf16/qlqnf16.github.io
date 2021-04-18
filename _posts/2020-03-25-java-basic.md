---
layout: post
title: "JAVA 기본"
categories: [dev, java]
---

객체지향프로그램 JAVA에 대해. 강의에서 이것저것 정리

### 데이터타입

- 기본 데이터타입
  - byte short int ...

instanceof 연산자: 객체의 상속관계 여부 확인 (return boolean)

## 클래스

### 생성자

클래스 생성시에 필수적인 메서드. 클래스명과 같은 이름으로, 리턴타입을 명시하지 않고 작성한다. 매개변수가 있는 생성자함수와 없는 생성자함수를 동시에 만들 수 있다 (method overloading).

```java
package pk03;

public class ClassPractice {
	private int aa;
	private int bb;

	public ClassPractice() {
		aa = 0; bb = 0;
	}

	public ClassPractice(int a, int b) {
		aa = a;
		bb = b;
	}

	public static void main(String[] args) {
		ClassPractice testClassPractice = new ClassPractice(10, 20);
		ClassPractice test2 = new ClassPractice();
		System.out.println(testClassPractice.aa);
		System.out.println(test2.aa);
	}
}

```

### this

클래스가 자기자신을 가르키기 위한 문법구조. this 레퍼런스의 역할은 두가지로, 1) 파라미터 값으로 멤퍼필드 초기화 2) 자신의 클래스 전체를 매개변수로 전달하는 경우 에 사용된다.

this() 는 생성자 안에서 또 다른 생성자를 호출할 때 사용된다. 생성자 안에서 반드시 첫번째 줄에 기술된다.

### garbage collecting

프로그램에서 더이상 객체가 사용되지 않게 되면 실행중에 차지하고 있던 시스템 메모리 리소스를 다시 반환시켜야 하는데, 이를 **garbage collecting**이라고 한다.

garbage collecting이 수행되는 시점은 정확히 알 수 없지만, <u>시스템이 한가해질 때</u> 수행된다고 한다. 자동적으로 garbage collecting이 수행되는 것이 대부분이나, 일부 자원이 제대로 반환되지 않을 때를 대비해 `finalize()` 메소드를 제공한다. `finalize()`는 객체의 레퍼런스에 null 값을 넣으면 자동으로 호출된다.

### 메소드 오버로딩

이름이 같은 메소드를 선언하는것. 데이터 타입이 다르거나 매개변수 리스트가 다르다.

```java
public int sum(int a, int b) {return a + b};
public int sum(int a, int b, int c) {return a + b + c};
public double sum(double a, double b) {return a + b};
```

### 상속

클래스는 다른 클래스를 상속받아, 상속받은 클래스의 메소드나 변수를 이용할 수 있다. 이 때 상속을 해주는 클래스를 부모, 상속받는 클래스를 자식 클래스라고 부른다. 상속관계는 계속 이어질 수 있으며, `class Berry extends Fruit` , `class Strawberry extends Berry` 와 같은 형태로 상속을 표현한다.

상속받은 클래스가 부모 클래스 내부의 메소드와 같은 이름의 메소드를 선언할 수 있다. 이때는 부모 클래스의 메소드가 부모 클래스의 메소드를 덮고 작동한다 (method overriding).

### super 레퍼런스

super 클래스, 즉 부모클래스를 가리키는 레퍼런스. super()는 부모클래스의 생성자를 호출한다. this와 비슷한 역할을 하는데, this는 자기자신을 super는 부모를 가리키는 점에서 다르다.

주의할 점은 this와 super는 static 메소드에서는 사용할 수 없다. 대표적으로 main 함수에서는 this, static을 사용할 수 없다. super 레퍼런스는 부모클래스와 자식클래스가 서로 같은 이름의 멤버필드를 가지고 있을 때 슈퍼클래스의 멤버필드에 접근하기 위해 유용하게 사용된다.

### 인터페이스

한 클래스가 여러개의 클래스로부터 상속받는 것을 다중상속이라고 하는데, 자바는 형식적으로 다중 상속을 허용하지 않는다. 다만 간접적으로 이를 허용하는데, **인터페이스(interface)**라는 개념으로 제공한다.

인터페이스는 명세만 기술해놓은 클래스로, 인터페이스 안에 선언된 메소드는 모두 추상메소드이다. 메소드는 항상 public abstract, 멤버필드는 항상 public static final이다. 따라서 이를 구현하는 클래스에서 해당 메소드들을 반드시 모두 오버라이딩 해줘야 한다.

여러개의 인터페이스를 동시에 상속받을 수 있는데, 이 때 각 인터페이스 내부의 메소드들의 이름이 겹치지 않아야 한다.

```java
public class clasA extends clasB implements clsC, clsD, clsE {}
```

### abstract, final, static

- abstract

  추상 클래스. 객체를 생성할 수 없고, 메소드가 선언만 되어있는 클래스다. 반드시 자식클래스가 상속받은 후 메소드를 오버라이딩해서 사용해야 한다.

- final

  상속받는 것은 가능하나 상속하는 것은 불가능한 클래스. final method는 오버라이딩 되지 않는다. 멤버필드 또한 값을 변경할 수 없다. 일반적으로 static 을 함께 붙여 선언한다.

- static

  메소드나 멤버필드가 오버라이딩 되지 않는다. 전역스코프를 가지고 있어 클래스 변수라고 불리기도 한다. 또한 클래스 내에서 static 을 이용해 초기화블록을 작성할 수 있다.

  ```java
  package pk01;

  class StVal {
  	int a;
  	static int b;
  }

  public class StaticTest {

  	public static void main(String[] args) {
  		// TODO Auto-generated method stub
  		StVal obj1 = new StVal();
  		StVal obj2 = new StVal();

  		obj1.a = 10;
  		obj1.b = 20;
  		System.out.println(obj1.a+" "+obj1.b); // 10 20 출력

  		obj2.a = 100;
  		obj2.b = 200;
  		System.out.println(obj2.a+" "+obj2.b); // 100 200 출력
  		System.out.println(obj1.a+" "+obj1.b); // 10 200 출력
  	}
  }
  ```

### 내부 클래스

클래스 안에 다시 클래스를 선언할 수 있는데, 이 때 안쪽에 있는 클래스를 내부 클래스, nested class라고 한다.

- static 내부 클래스

  객체를 바깥 클래스에서 독립적인 객체로 생성하고자 할 때 사용. 다음과 같이 객체를 생성, 사용한다.

  ```java
  package pk01;

  class ClsOut {
  	  static int a = 1;
  		public static class ClsIn {
  			String Infunc() {
  				return (a+"번째 static 내부 클래스");
  			}
  		}
  	}

  public class StaticTest {

  	public static void main(String[] args) {
  		String string;
  		ClsOut.ClsIn objClsIn = new ClsOut.ClsIn();

  		string = objClsIn.Infunc();
  		System.out.println(string);
  	}

  }
  ```

- non-static 내부클래스

  먼저 바깥 클래스의 객체가 생성되어야 한다. 따라서 만약 위에서 ClsIn이 non-static 내부 클래스로 선언되었다면, 다음과 같이 생성해 사용할 수 있다.

  ```java
  public static void main(String[] args) {
  		String string;
  		ClsOut objClsOut = new ClsOut();
  		ClsOut.ClsIn obj2 = objClsOut.new ClsIn();
  		string = obj2.Infunc();
  		System.out.println(string);
  	}
  ```

- 지역 내부클래스

  static, non-static 내부클래스는 바깥클래스의 멤버의 위치에 선언되는 반면, 지역내부클래스는 바깥 클래스의 메소드 내부에서 선언된다.

- 무명내부클래스

  클래스 이름을 갖지 않고 단 한번만 사용할 수 있다. 주로 이벤트 처리에 많이 이용된다.

  ```java
  button.addActionListener {
  	new ActionListener() {
  		public void actionPerformed(ActionEvent e) {
  			//
  		}
  	}
  }
  ```

### 자바의 주요 클래스

1. **Type Wrapper 클래스**

자바에서는 기본 데이터타입을 객체로 전환해 사용해야 할 때가 있다. 예를 들어 어떤 메소드가 매개변수로 객체만 받는 경우처럼. 이 때 사용하는 것이 type wrapper class이다. 일반적으로 기본데이터타입의 맨 첫글자를 대문자로 표현된 것이 Type Wrapper class이다. (ex. byte => Byte)

2. **String / StringBuffer / StringTokenizer**

- String: 문자열을 저장하는 클래스. 값을 수정할 수 없으므로 정적 클래스라고도 한다.
- StringBuffer: 문자열을 수정할 수 있는 클래스. String 클래스와 대조돼 동적문자열이라 한다. String 클래스에 저장된 값을 변경하기 위해 사용한다.
- StringTokenizer: 문자열을 토큰 단위로 분리하는 클래스. 토큰은 delimiter(`,`, `.`, `" " `) 단위로 분리된다. java.util package에 포함되어 있다.

3. **Vector**

c++의 vector와 같음. 동적배열을 구현하기 위해 사용한다. 모든 클래스형의 객체가 Vector에 저장될 수 있다. 주의할 점은 **기본데이터타입(byte, int, float 등)은 사용할 수 없으며**, wrapper class(Byte, Int, Float 등)로 사용해야 한다는 것이다.

```java
Vector<Object>< vec = new Vector<Object>(2);
```

위와 같이 선언하면 어떤 자료형이든 넣어서 사용할 수 있다. 2는 vector의 초기 크기(capacity)를 설정하는 숫자값인데, 비워둘 경우 기본값은 10이다. 벡터의 크기를 초과할 경우 크기는 두배씩 늘어난다.

4. **Dictionary, Hashtable, Properties**

Dictionary는 추상클래스로서, Hashtable의 부모클래스이다. key-value 쌍으로 구성되어 있다. Properties 클래스는 Hashtable 클래스의 자식 클래스로, 파일로 저장된 key, value를 읽어들이거나, key-value 쌍을 파일로 저장할 수 있는 객체이다. java.util package에 포함되어 있다.

## JAVA Collections

**<u>객체들의 모임.</u>** Collection Framework를 이용하면 일반 배열보다 더 다양한 처리를 쉽게 할 수 있다. 위에서 언급된 Vector, Hashtable, Stack 또한 일종의 컬렉션이다. Collection Framework는 인터페이스 또는 클래스 형태로 제공되며, **List, Set, Map, Iterator** 등이 대표적이다.

1. Array: 대표적 Collection Framework 클래스.
2. List: 중복 요소를허용하는 정렬된 데이터 구조 인터페이스. 인터페이스이므로 자식클래스 객체를 생성해 사용한다.

```
// 리스트 구조를 갖는 ArrayList 클래스 객체 생성
List<String> ls = new ArrayList<String>();
```

3. Set: 중복 요소를 허용하지 않음. HashSet, TreeSet, LinkedHashSet 중 하나의 자식클래스 객체를 생성해 사용한다.
4. Map: key-value를 연결하는 컬렉션. HashMap, TreeMap, LinkedHashMap 중 하나의 자식클래스 객체를 생성해 사용한다.
