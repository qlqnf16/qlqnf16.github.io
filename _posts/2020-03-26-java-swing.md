---
layout: post
title: "Swing"
categories: [dev, java]
---

swing 컴포넌트란 무엇일까?

## swing component

자바의 JFC(Java Foundation Class)는 **GUI 프로그래밍**에 필요한 각종 킷을 모아놓은 것으로, 현재는 GUI의 기능들을 구현할 수 있는 스윙, 2D, Drag&Drop 등을 지원한다. 스윙은 AWT와 유사하나, AWT보다 가볍고 더 많은 컴포넌트와 기능을 지원한다. 또한 자바 자체적으로 제작된 컴포넌트이므로, 플랫폼에 관계없이 동일한 모양으로 사용할 수 있다.

### container

- 애플리케이션 Jframe

  컴포넌트를 담는 컨테이너 역할. 자바로 어플리케이션을 작성할 때 사용, main() 이 반드시 존재한다.

  마지막에 반드시 `EXIT_ON_CLOSE`를 적용해야 프로그램이 백그라운드에서 불필요하게 동작하지 않는다.

- 애플릿 JApplet

  웹브라우저에저 기반으로 동작하는 프로그램. init() 메소드로 시작한다. 애플릿 메소드의 호출 순서는 `init() -> start() -> [paint()] -> stop() -> destroy()` 이다.

  웹브라우저 기반으로 도작하므로 html 코드에 끼워서 사용한다.

### components

- JLabel() - 레이블을 생성. 비거나, 텍스트, 이미지를 내용으로 가지는 레이블 객체를 생성할 수 있다.

- JTextField(), JPasswordField() - 각각 텍스트입력상자, 비밀번호 입력상자를 생성

- JButton() - 버튼 생성. `Jbutton button = new JButton("확인")` 과 같은 형태로 사용할 수 있다.

- JTextArea() - JTextField는 한 줄만 입력가능하지만, JTextArea는 여러줄을 입력할 수 있는 텍스트 상자를 생성한다. 3줄 이상으로 넘어가면 스크롤바가 필요한데, 스크롤바 객체 또한 다음과 같이 생성해서 사용할 수 있다.

  ```java
  JTextArea textarea = new JTextArea(3, 30);
  JScrollPane scr = new JScrollPane(textarea);
  ```

- JCheckBox(), JRadioButton() - 각각 중복선택이 가능/불가능한 선택 박스를 생성한다. JRadioButton()은 중복선택이 불가능하기 때문에, 다음과 같이 논리적 그룹을 만들어 감싸줘야 한다.

  ```java
  JRadioButton rab1 = new JRadioButton("M", true);
  JRadioButton rab2 = new JRadioButton("F", false);
  ButtonGroup group = new ButtonGroup();
  group.add(rab1);
  group.add(rab2);
  ```

- JComboBox(), JList() - 리스트 형의 선택옵션을 생성한다. 콤보박스와 리스트의 차이는 다음과 같다고 한다.

  > **Combo Box**
  >
  > 1. 일반적으로 생각하는 리스트 선택 상자의 형태. 선택된 값 하나만 보인다.
  > 2. 하나의 값만 선택할 수 있다.
  > 3. 리스트 내부에 체크박스가 들어올 수 없다.
  >
  > **List Box**
  >
  > 1. 여러 값을 한 번에 보여준다.
  > 2. 여러 값을 중복해 선택할 수 있다.
  > 3. 리스트 내부에 체크박스가 들어올 수 있다.

### Layout Manager

자바 GUI는 컴포넌트들을 컨테이너에 추가해 구성되는데, 이 때의 **모양을 결정하는 것이 Layout Manager**이다. `conatiner.setLayout(new FlowLayout());`과 같은 형태로 사용할 수 있다.

- **FlowLayout**: 왼쪽->오른쪽 순으로 배치. 한줄이 다 차면 다음줄로 이동해서 배치. 기본 정렬은 가운데정렬로, `new FlowLayout(FlowLayout, LEFT)` 와 같이 정렬을 바꿀 수 있다.

- **BorderLayout**: center/north/east/west/south 등 정해진 위치에만 위치시킬 수 있는 레이아웃.

  ```java
  JButton b1 = new JButton("1");
  JButton b2 = new JButton("2");
  container.add(b1, BorderLayout.NORTH);
  container.add(b2, BorderLayout.EAST);
  ```

- **GridLayout**: 3x3 등 그리드를 만들어 그 안에 컴포넌트를 배치한다.

  ```java
  conatiner.setLayout(new GridLayout(4, 3));   // 행, 열 순
  container.add(b1, BorderLayout.NORTH);
  container.add(b2, BorderLayout.EAST);
  ```

- **CardLayout**: 컨테이너 내부의 컴포넌트를 하나의 카드로 취급, 한 번에 하나의 카드만을 보여주는 레이아웃. 한 페이지에서 다른 이미지를 번갈아 출력하거나 하는 상황에 쓰인다.
- **BoxLayout**: 컴포넌트들이 수직/수평 일렬로 배치하는 레이아웃. FlowLayout과 달리 줄 끝에 닿아도 줄바꿈을 하지 않는다.

### 기타 기능

- **JMenuBar**

  JFrame, JApplet 컨테이너에 `setMenuBar()` 메소드를 통해 컨테이너 메뉴를 추가할 수 있다.

  ```java
  public MenuTest() {
    JMenuBar bar = new JMenuBar();
    setJMenuBar(bar);

    JMenu fileMenu = new JMenu("File");
    fileMenu.setMnemonic('F'); //단축키 설정
    JMenuItem newItem = new JMenuItem("New");
    newItem.setMnemonic('N');
    fileMenu.add(newItem); // 아이템을 메뉴에 추가
    JMenuItem openItem = new JMenuItem("Open");
    openItem.setMnemonic('O');
    fileMenu.add(openItem);
    bar.add(fileMenu); // 메뉴바에 메뉴 추가

    setLocation(800, 200);
    setSize(300, 300);
    setVisible(true);
  }
  ```

- **JPopupMenu**

  팝업메뉴는 마우스 오른쪽 버튼을 누르는 이벤트에 반응해 화면에 표시되는 메뉴다. 따라서 이벤트리스너를 생성해 함께 사용해야 한다.

- **JTable**: 테이블 형태로 자료를 표현할 때 사용. `JTable ta = new JTable(dat, title)` 과 같은 형태로 구현한다. `dat`는 테이블의 필드명을 가지는 레퍼런스, `title`은 테이블의 레코드를 가지는 레퍼런스이다.

- **JTabbedPane**: 컴포넌트를 탭의 형태로 구성할 수 있게 한다. 다음과 같이 JTappedPane을 생성하고 그 안에 컴포넌트를 넣어서 사용할 수 있다.

  ```java
  JTabbedPane tab = new JTabbedPane();
  JPanel panel = new JPanel();
  tab.add("기록", new JTextArea());
  tab.add("수정", panel);
  ```

### Event and Event Listener

마우스 클릭, 버튼 클릭, 키보드 입력 등의 동작은 **이벤트를 발생**시키고, 그 이벤트에 따라 특정 동작을 처리할 수 있게 하는 것이 **이벤트 리스너**이다. 리스너는 컴포넌트의 이벤트를 감지해 이를 처리하는 메소드로 연결시키는 인터페이스이다. 또한 자바에서 **이벤트핸들러의 리턴 타입은 항상 void** 형이어야 한다.

Listener는 **추상메소드**로, implement 해서 사용시 **반드시 오버라이딩 해야 한다.** 만약 메소드 중 일부만 필요한 경우, Listener 인터페이스를 implement 하는 것보다 **Adapter 클래스**를 상속받아 사용하는 것이 더 좋을 수 있다.

- **ActionEvent** - 버튼 클릭 / 엔터키 / 메뉴 아이템을 선택할 때 발생하는 이벤트
  반드시 `actionPerformed()`를 오버라이딩 해서 사용해야 한다,
- **ItemEvent** - 체크박스, 라디오버튼, 콤보박스의 항목을 선택/해제할 때 발생하는 이벤트
  반드시 `itemStateChange()`를 오버라이딩 해서 사용해야 한다.
- **MouseEvent** - 마우스 동작에서 발생하는 이벤트. 크게 마우스 클릭 / 이동으로 나뉜다. 마우스 클릭 이벤트는 `MouseListener`가, 마우스 이동 이벤트는 `MouseMotionListener` 가 감지한다.
  - **MouseListener** - `mousePressed()`, `mouseClicked()`, `mouseExited()` 등
  - **MouseMotionListener** - `mouseDragged()`, `mouseMobed()` 등
- 이 외에도 WindowEvent(창 변화), ListSelectionEvent, KeyEvent, ChangeEvent(상태 변화) 등 다양한 이벤트를 사용할 수 있다.

```java
package pk31;
import java.awt.*;
import javax.swing.*;
import java.awt.event.*;

public class EventTest extends JFrame {
	private static final long serialVersionUID = 1L;
	JCheckBox checkBox;
	public EventTest() {
		Container container = getContentPane();
		container.setLayout(new FlowLayout());
		checkBox = new JCheckBox("agree");
		container.add(checkBox);

		CheckBoxHandler handler = new CheckBoxHandler();
		checkBox.addItemListener(handler);

		addMouseListener(new MouseAdapter() {
      // adapter 클래스를 상속, 필요한 메소드 오버라이딩
			public void mousePressed(MouseEvent e) {
				JOptionPane.showMessageDialog(null, "you clicked");
			}
		});

		setLocation(600, 200);
		setSize(300, 300);
		setVisible(true);
	}

  // ItemListener를 implement 해서 사용
	private class CheckBoxHandler implements ItemListener{
		public void itemStateChanged(ItemEvent e) {
      // itemStateChanged 메소드 오버라이딩
			JOptionPane.showMessageDialog(null, "You agreed");
		}
	}

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		EventTest objEventTest = new EventTest();
		objEventTest.setDefaultCloseOperation(EXIT_ON_CLOSE);
	}
}
```
