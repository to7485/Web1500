### 스윙
- 스윙은 보다 세련된 형태의 GUI를 제공하기 위해서 만들어진 UI 클래스들의 모음

### 프레임 : JFrame

#### JFrame의 생성자
|생성자|설명|
|---|---|
|JFrame()|보이지 않는 JFrame을 생성한다.|
|JFrame(String title)|타이틀을 지정한 보이지 않는 JFrame을 생성한다.|
- 프레임을 생성했을때는 기본적으로 화면에 보이지 않는다.
- 화면에 프레임을 표시하고자 할 때는 위치와크기, 화면에 표시할 것인지에 대한 여부를 지정해줘야 한다.
- setSize(int width, int height) : 프레임의 크기 지정
- setLocation(int x, int y) : 프레임이 보일 좌표설정
- setVisible(boolean value) : 화면 표시 여부 설정

#### JFrame의 주요 메서드
|메서드|설명|
|---|---|
|void add(Component c)|지정된 컴포넌트를 패널에 추가한다.|
|JMenuBar getJMenuBar()|지정된 JMenuBar를 구한다.|
|void pack()|프레임에 부속된 컴포넌트들의 크기에 맞게 프레임의 크기를 조절한다.|
|void remove(Component c)|프레임에 지정된 컴포넌트를 제거한다.|
|void setDefaultCloseOperation(int operation)|사용자가 JFrame을 닫을 때 기본적으로 어떤 작업을 할지 정한다. operation은 하기 표 참조|
|void setIconImage(Icon image)|프레임이 최소화 되었을 떄의 아이콘을 지정한다.|
|void setLayout(LayoutManager layout)|배치관리자를 지정한다(디폴트는 BoarderLayout 배치관리자이다.)|
|void setLocation(int x, int y)|프레임의 x좌표, y좌표를 지정한다.|
|void setResizable(boolean value)|프레임의 크기 변경 허용 여부를 지정한다.|
|void setSize(int width, int height)|프레임의 크기를 설정한다.|
|void setJMenuBar(JMenubar menubar)|	현재 프레임에 메뉴바를 붙인다.|

#### setDefaultCloseOperation(int operation) 메서드의 operation 값
|상수값|설명|
|---|---|
|EXIT_ON_CLOSE|닫기 단추를 누르면 창을 화면에서 사라지게 하고, 메모리에도 제거된다.|
|DO_NOTHING_ON_CLOSE|닫기 단추를 비활성화 시킨다. 이 상수는 JFrame에서 제공하는 적업을 하지 않고 등록된 WindowListener의 windowClosing()메서드에서 원하는 작업을 하고 싶을 때 사용한다.|
|HIDE_ON_CLOSE|등록된 WindowListener의 메서드를 호출하기 전에 자동적으로 JFrame을 닫는다. 화면에서 창을 숨겨준다.|
|DISPOSE_ON_CLOSE|닫기 단추를 누르면 창을 화면에서 사라지게 하고, 메모리에는 제거되지 않는다.|

### 스윙의 주요 컴포넌트 클래스

|클래스|설명|
|---|---|
|AbstractButton|버튼과 연관된 클래스들의 상위 추상 클래스|
|ButtonGroup|버튼을 그룹화하기 위한 클래스|
|ImageIcon|이미지를 아이콘으로 캡슐화하여 제공하는 클래스|
|JButton|버튼 클래스|
|JCheckBox|체크박스 클래스|
|JCheckBoxMenuItem|체크박스의 메뉴 아이템 클래스|
|JColorChooser|컬러 선택 다이얼로그 클래스|
|JComponent|모든 스윙 컴포넌트의 최상위 클래스|
|JLabel|레이블 클래스|
|JList|리스트 클래스|
|JMenu|메뉴 클래스|
|JMenuBar|메뉴 바 클래스|
|JMenuItem|메뉴 아이템 클래스|
|JPanel|패널 클래스|
|JPasswordField|패스워드 입력 클래스|
|JPopupMenu|팝업 메뉴 클래스|
|JProgressBar|진행 바 클래스|
|JRadioButton|라디오 버튼 클래스|
|JRadioButtonMenuItem|메뉴에 사용하는 라디오 버튼 클래스|
|JScrollBar|스크롤바 클래스|
|JScrollPane|스크롤 윈도우를 나타내는 클래스|
|JSeparator|분리자 클래스|
|JSlider|슬라이더 클래스|
|JTabbedPane|그룹을 폴더 형태로 제공하는 윈도우를 나타내는 클래스|
|JTable|테이블 클래스|
|JTableHeader|테이블 헤더 클래스|
|JTextArea|텍스트 에어리어 클래스|
|JTextComponent|텍스트 관련 클래스들의 상위 클래스|
|JTextField|텍스트 필드 클래스|
|JToggleButton|토글 버튼 클래스|
|JToolBar|툴바 제공 클래스|
|JToolTip|풍선 도움말 클래스|
|JTree|트리형태를 나타내는 클래스|

ex1_JFrame 패키지 생성

#### Ex1_JFrame 클래스 생성
```java
package test;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.URL;

import javax.swing.JFrame;

public class JFrameTest extends JFrame {
	
	public JFrameTest() {
		super("제목");
		setBounds(300,300,300,200);
		setVisible(true);
	}
	
	public static void main(String[] args) {
		JFrameTest f = new JFrameTest();
		f.setDefaultCloseOperation(EXIT_ON_CLOSE);
	}
}
```

### JButton
- JButton은 클릭 기능을 제공한다.
- JButton클래스는 문자열 또는 아이콘을 사용하여 버튼을 생성할 수가 있으며, AbstractButton 클래스로부터 상속받는다.|

#### JButton 클래스의 생성자

|생성자|설명|
|-----|------|
|JButton()|text와 아이콘을 사용하지 않는 버튼을 생성한다.|
|JButton(Icon icon)|아이콘을 가진 버튼을 생성한다.|
|JButton(String text)|문자열 기반인 text를 가진 버튼을 생성한다.|
|JButton(String text, Icon icon)|문자열인 text와 아이콘을 가진 버튼을 생성한다.|

#### JButton 클래스의 메서드

|메서드|설명|
|-----|------|
|boolean isDefaultButton()|이 버튼이 RootPane의 기본 버튼인지 알아낸다.|
|boolean isDefaultCapable()|이 버튼이 RootPane의 기본 버튼이 될 수 있는지 알아낸다.|
|void setDefaultCapable(boolean defaultCapable)|이 버튼이 RootPane의 기본 버튼이 될 수 있는지의 여부를 정한다.|

#### Ex2_JButton
```java
package test;

import java.awt.FlowLayout;

import javax.swing.JButton;
import javax.swing.JFrame;

public class JButtonTest extends JFrame {
	JButton jbtn1, jbtn2, jbtn3;
	JButtonTest() {
		super("버튼(JButton) 추가");
		setLayout(new FlowLayout());
		
		jbtn1 = new JButton("1");
		jbtn2 = new JButton("2");
		jbtn3 = new JButton("3");
		
		add(jbtn1);
		add(jbtn2);
		add(jbtn3);
		
		setSize(300, 200);
		setVisible(true);
		setDefaultCloseOperation(EXIT_ON_CLOSE);
	}
	
	public static void main(String[] args) {
		new JButtonTest();
	}

}

```
![image](https://user-images.githubusercontent.com/54658614/223491878-09f2725f-b9c2-4caf-b77f-d8085d7216b1.png)

## 이벤트와 이벤트 처리의 개념 
- 이벤트는 사용자의 입력, 키보드나 마우스 등의 장치나 소프트웨어적으로 발생하는 모든 사건을 의미한다.
- 이벤트가 발생하면 발생된 이벤트에 반응하여 필요한 것을 처리하는데 이를 <b>이벤트 핸들러(event handler)</b>이다.
- 자바에서 이벤트 핸들어는 메소드로 구현되며, 이벤트의 동작에 등답하는 방식으로 처리되는 프로그램을 이벤트 지향(event-driven) 프로그램이라 한다. 
- 이벤트 지향 프로그램은 무한 루프를 돌면서 사용자의 이벤트가 발생하기를 기다린다. 사용자의 이벤트가 밸생하면 이벤트를 처리하고 다시 무한 루프로 대기한다.

#### AWT 이벤트 관련 클래스
- 자바에서 각 컴포넌트들은 다양한 종류의 이벤트를 발생시키며, 이벤트는 객체로 처리한다.
- 스윙은 AWT에서 제공하는 이벤트(java.awt.event 패키지로 제공)를 처리할 수 있을 뿐 아니라 스윙 컴포넌트에서 발생하는 이벤트를 따로 정의해 두었다.
- 스윙의 이벤트 클래스는 javax.swing.event 패키지에서 제공한다.

#### 컴포넌트별 발생 이벤트

![이벤트2](https://raw.githubusercontent.com/yonggyo1125/curriculum300H/main/1.JAVA(84%EC%8B%9C%EA%B0%84)/23%EC%9D%BC%EC%B0%A8(3h)%20-%20%EC%82%AC%EC%9A%A9%EC%9E%90%20%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4(%EC%8A%A4%EC%9C%99)/images/%EC%9D%B4%EB%B2%A4%ED%8A%B82.png)

- 자바에서 대부분의 이벤트는 사용자가 GUI화면에서 마우스나 키보드를 조작함으로써 발생한다.
- 이벤트의 발생은 마우스나 키보드를 조작함으로서 이루어지지만, 실제 이벤트 객체를 생성시키는 역할은 JVM에 의해서 이루어진다. 즉, JVM은 사용자의 조작을 인지하고 조작과 관련된 이벤트 객체를 이벤트 클래스로부터 자동 생성한다.

#### 이벤트의 종류

<table>
<tr>
	<thead>
		<tr>
			<th>구분</th>
			<th>이벤트</th>
			<th>설명</th>
		</tr>
	</thead>
	<tbody>
	<tr>
		<td rowspan='10'>AWT</td>
		<td>ActionEvent</td>
		<td>버튼이 눌려지거나 리스트나 메뉴의 항목이 선택되었을 경우에 발생</td>
	</tr>
	<tr>
		<td>AdjustmentEvent</td>
		<td>스크롤바를 움직였을 떄 발생</td>
	</tr>
	<tr>
		<td>ComponentEvent</td>
		<td>컴포넌트를 선택하거나 조절했을 때 발생</td>
	</tr>
	<tr>
		<td>ContainerEvent</td>
		<td>컨테이너에 컴포넌트가 추가, 제거되었을 때 발생</td>
	</tr>
	<tr>
		<td>FocusEvent</td>
		<td>키보드 입력을 받아들일 수 있는 초점(focus)을 획득하거나 읽었을 때 발생</td>
	</tr>
	<tr>
		<td>ItemEvent</td>
		<td>체크박스, 리스트, 메뉴의 항목이 선택, 해제되었을 때 발생</td>
	</tr>
	<tr>
		<td>KeyEvent</td>
		<td>키보드로 부터 입력이 일어났을 때 발생</td>
	</tr>
	<tr>
		<td>MouseEvent</td>
		<td>마우스의 행위가 일어났을 떄 발생</td>
	</tr>
	<tr>
		<td>TextEvent</td>
		<td>텍스트 필드나 에이러어 값에 입력될 때 발생</td>
	</tr>
	<tr>
		<td>WindowEvent</td>
		<td>윈도우가 활성화, 비활성화, 아이콘화, 아이콘 복구, open, close 등에서 발생</td>
	</tr>
	<tr>
		<td rowspan='13'>스윙</td>
		<td>CaretEvent</td>
		<td>컴포넌트에 있는 텍스트의 캐럿이 변할 경우에 발생</td>
	</tr>
	<tr>
		<td>ChangeEvent</td>
		<td>컴포넌트의 상태에 변화가 생길 경우 발생</td>
	</tr>
	<tr>
		<td>HyperlinkEvent</td>
		<td>하이퍼링크와 관련하여 무엇인가가 발생할 경우 발생</td>
	</tr>
	<tr>
		<td>ListSelectionEvent</td>
		<td>리스트의 선택과 관련해서 변화가 생길 경우 발생</td>
	</tr>
	<tr>
		<td>MenuDragMouseEvent</td>
		<td>마우스가 드래그 되는 상태에서 메뉴구성요소가 마우스 이벤트를 받을 때 발생</td>
	</tr>
	<tr>
		<td>MenuEvent</td>
		<td>메뉴가 선택, 축소될 때 발생</td>
	</tr>
	<tr>
		<td>MenuKeyEvent</td>
		<td>메뉴 트리에서 메뉴 구성요소가 키 이벤트를 받을 때 발생</td>
	</tr>
	<tr>
		<td>PopupMenuEvent</td>
		<td>팝업 메뉴 안에 있는 컴포넌트의 변화에 발생</td>
	</tr>
	<tr>
		<td>TableColumnModelEvent</td>
		<td>테이블 칼럼에 변화가 생길 때 발생</td>
	</tr>
	<tr>
		<td>TableModelEvent</td>
		<td>테이블 모델에 변화가 생길 때 발생</td>
	</tr>
	<tr>
		<td>TreeExpansionEvent</td>
		<td>트리에서 한 개의 패스를 확인할 때 발생</td>
	</tr>
	<tr>
		<td>TreeModelEvent</td>
		<td>트리 모델에 변화가 생길 때 발생</td>
	</tr>
	<tr>
		<td>TreeSelectionEvent</td>
		<td>트리에서 선택의 변화가 생길 때 발생</td>
	</tr>
	</tbody>
</tr>
</table>

## 리스너 인터페이스를 이용한 이벤트 처리
- 리스너 인터페이스는 이벤트와 이벤트 핸들러 사이를 연결시켜주는 역할을 한다.
- 이벤트가 발생한 해당 컴포넌트를 리스너에 등록시켜야 프로그램의 제어가 해당 이벤트가 제공하는 이벤트 핸들러로 옮겨진다. 
- 자바에서 이벤트 핸들러는 메서드로 표현되며, 이벤트를 처리하는 메서드이다.

- 리스너 인터페이스를 이용하여 이벤트를 처리하기 위해서는 다음과 같은 순서로 프로그램을 작성해야 한다.
	1. 발생하는 이벤트를 처리할 이벤트 종류 결정
	2. 이벤트 처리에 적합한 리스너 인터페이스를 사용하여 클래스 작성
	3. 이벤트를 받아들일 각 컴포넌트에 리스너 등록
	4. 리스너 인터페이스에 선언된 메서드를 오버라이딩하여 이벤트 처리 루틴 작성(이벤트 핸들러 작성)
	
- AWT에서 제공하는 java.awt.event 패키지와 스윙에서 제공하는 javax.swing.event 패키지는 각각의 이벤트 클래스와 연관된 이벤트 리스너 인터페이스를 제공하고 있다.
- 이벤트 리스너 인터페이스는 사용자로부터 받아들인 이벤트를 처리할 메소드들을 선언하고 있다.
- 프로그램에는 이벤트 처리를 위해 이벤트 리스너 인터페이스에서 선언된 모든 메서드들을 오버라이딩하여 구현하여야 한다.

#### 이벤트 클래스와 리스터 인터페이스

|이벤트 클래스|리스너 인터페이스|리스너 인터페이스 메소드(이벤트 핸들러)|
|------|-----|------|
|ActionEvent|ActionListener|actionPerformed(ActionEvent e)|
|ChangeEvent|ChangeListener|stateChanged(ChangeEvent e)|
|ItemEvent|ItemListener|itemStateChanged(ItemEvent e)|
|KeyEvent|KeyListener|keyPressed(KeyEvent e)<br>keyReleased(KeyEvent e)<br>keyTyped(KeyEvent e)|
|ListSelectionEvent|ListSelectionListener|valueChanged(ListSelectionEvent e)|
|MouseEvent|MouseListener|mouseClicked(MouseEvent e)<br>mouseEntered(MouseEvent e)<br>mouseExited(MouseEvent e)<br>mousePressed(MouseEvent e)<br>mouseReleased(MouseEvent e)|
|MouseEvent|MouseMotionListener|mouseDragged(MouseEvent e)<br>mouseMoved(MouseEvent e)|
|WindowEvent|WindowListener|windowActivated(WindowEvent e)<br>windowClosed(WindowEvent e)<br>windowClosing(WindowEvent e)<br>windowDeactivated(WindowEvent e)<br>windowDeiconified(WindowEvent e)<br>windowIconified(WindowEvent e)<br>windowOpened(WindowEvent e)|

### ActionEvent
- ActionEvent는 버튼을 눌렀을 때, 텍스트 필드에서 엔터키를 눌렀을 때, 리스트 항목이 선택되었을 때, 메뉴의 한 항목이 선택되었을 때 발생한다.

#### ActionEvent 생성자

|생성자|설명|
|-----|------|
|ActionEvent(Object src, int type, String cmd)|src는 이벤트를 발생한 객체, type은 이벤트 타입을, cmd는 이벤트를 발생시킨 컴포넌트의 테이블을 의미한다.|
|ActionEvent(Object src, int type, String cmd, int modifiers)|src는 이벤트를 발생한 객체, type은 이벤트 타입을, cmd는 이벤트를 발생시킨 컴포넌트의 레이블을, modifiers는 이벤트가 발생할 때 같이 사용된 수정자의 키의 상수를 의미한다.|

- ActionEvent는 마우스를 누를 때 같이 사용된 수정자(modifier)키가 있는 경우에 구분하기 위하여 4개의 상수를 제공합니다.

#### 수정자 키

|수정자 키|설명|
|-----|------|
|ALT_MASK|수정자 키로 ALT키를 사용|
|CTRL_MASK|수정자 키로 CTRL 키를 사용|
|META_MASK|수정자 키로 META 키를 사용|
|SHIFT_MASK|수정자 키로 SHIFT 키를 사용|

#### ActionEvent의 메서드

|메서드|설명|
|----|------|
|String getActionCommand()|ActionEvent를 발생시킨 객체의 이름을 구한다.|
|int getModifiers()|ActionEvent 발생 시에 같이 사용된 수정자 키(ALT, CTRL, META, SHIFT)를 나타내는 상수값을 구한다.|
|String getSource()|이벤트를 발생시킨 이벤트 소스 객체(이벤트를 발생시킨 컴포넌트)를 구한다.|

- ActionEvent는 ActionListener 인터페이스를 구현(implements)하여 사용하는데, ActionListener가 가지고 있는 메서드인 actionPerformed(ActionEvent e)를 반드시 오버라이딩 해주어야 한다.

```java
package test;

import java.awt.FlowLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JOptionPane;

public class Test{
	public static void main(String[] args) {
		JFrame f = new JFrame("ActionEvent 처리");
		f.setLayout(new FlowLayout());
		
		//버튼 객체 생성하기
		JButton jbtn1 = new JButton("입력");
		JButton jbtn2 = new JButton("확인");
		JButton jbtn3 = new JButton("옵션");
		JButton jbtn4 = new JButton("메시지");
		
		
		//버튼 프레임에 추가하기
		f.add(jbtn1);
		f.add(jbtn2);
		f.add(jbtn3);
		f.add(jbtn4);
		
		//버튼별 이벤트 만들기
		ActionListener al = new ActionListener() {
			
			@Override
			public void actionPerformed(ActionEvent e) {
				//get
				switch(e.getActionCommand()) {
				case "입력":
					String name = JOptionPane.showInputDialog(f,"이름을 입력하세요");
					System.out.println(name);
					break;
				case "확인":
					int con = JOptionPane.showConfirmDialog(f, "확인?");
					/** 옵션 타입은 YES_OPTION, YES_NO_OPTION, YES_NO_CANCEL_OPTION이 있다 */
					break;
				case "옵션":
					String[] option = {"검색", "추가", "취소"};
					JOptionPane.showOptionDialog(
						f, "원하는 직업 선택", "옵션 대화상자" ,
						JOptionPane.YES_NO_CANCEL_OPTION, JOptionPane.INFORMATION_MESSAGE, null, option, option[0]);
					break;
				case "메세지":
					JOptionPane.showMessageDialog(f, "메세지 대화상자");
					break;
				}
				
			}
		};
		
		//만든 이벤트 붙혀주기
		jbtn1.addActionListener(al);
		jbtn2.addActionListener(al);
		jbtn3.addActionListener(al);
		jbtn4.addActionListener(al);
		
		
		f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		f.setSize(300, 200);
		f.setVisible(true);
		
		
	}

}

```

### 패널 : JPanel
- JPanel은 JFrame에 붙히는 중간 컨테이너 역할을 한다.
- 화면이 복잡한 형태인 경우 요소를 그룹별로 묶어서 표현할 수 있는데, 이러한 경우 JPanel에다 묶어서 Frame에다 붙힐 수 있다.

#### JPanel의 생성자
|생성자|설명|
|---|---|
|JPanel()|레이아웃 매니저가 FlowLayout인 JPanel을 생성한다|
|JPanel(LayoutManger layout)|레이아웃 매니저가 layout인 JPanel을 생성한다.|

#### 배치관리자
- FlowLayout : 왼쪽에서 오른쪽으로 배치, 오른쪽 공간이 없으면 아래로 배치
- BorderLayout : 동,서,남,북,중앙 5개의 영역으로 나눠준다.
- GridLayout : 2차원 표 모양으로서 n X n으로 설정해주며 왼쪽에서 오른쪽, 위에서 아래 순으로 배치
- CardLayout : 컴포넌트를 포개어 배치
- Null : 레이아웃을 쓰지 않고 각 컴포넌트마다 수동으로 위치를 설정

#### JPanel의 주요 메서드
|메서드|설명|
|---|---|
|void add(Component c)|지정된 컴포넌트를 패널에 추가한다.|
|void remove(Component c)|패널에 지정된 컴포넌트를 제거한다.|
|void setLayout(LayoutManager layout)|배치관리자를 지정한다(디폴트는 FlowLayout 배치관리자이다.)|
|void setLocation(int x, int y)|패널의 x좌표, y좌표를 지정한다.|
|void setSize(int width, int height)|패널의 크기를 설정한다.|
|void setToolTipText(String text)|패널의 빈곳에 마우스를 올려놓으면 툴팁을 표시한다.|

#### Ex2_JPanel 클래스 생성

```java
package test;

import java.awt.Color;
import java.awt.FlowLayout;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JPanel;

public class Test extends JFrame {
	
	public Test() {
        super("FlowLayout 예제");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        
        JPanel p1 = new JPanel();
        p1.setBackground(Color.YELLOW);
        p1.setLayout(new FlowLayout());
        //p1.setLayout(new GridLayout(3,2));
	//p1.setLayout(new BorderLayout());
        p1.add(new JButton("1"));
        p1.add(new JButton("2"));
        p1.add(new JButton("3"));
        p1.add(new JButton("4"));
        p1.add(new JButton("5"));
        
        this.add(p1);
        setSize(300, 200);
        setVisible(true);
    }
    
    public static void main(String[] args) {
    	Test frame = new Test();
    	
    }
}

```
### FlowLayout()
![image](https://user-images.githubusercontent.com/54658614/223475784-5ac7e8ee-df9f-4538-8a32-8d447fcb07b8.png)

### GridLayout(3,2)
![image](https://user-images.githubusercontent.com/54658614/223476144-85248fb5-26e1-4fbb-9a52-39eece07a235.png)

#### Ex3_Panel클래스 생성
```java
package test;

import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.FlowLayout;
import java.awt.GridLayout;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JPanel;

public class Test extends JFrame {
	
	public Test() {
        super("FlowLayout 예제");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        
        JPanel p1 = new JPanel();
        p1.setBackground(Color.YELLOW);

        p1.setLayout(new BorderLayout());
	
        p1.add("North",new JButton("위"));
        p1.add("West",new JButton("왼쪽"));
        p1.add("Center",new JButton("가운데"));
        p1.add("East",new JButton("오른쪽"));
        p1.add("South",new JButton("아래"));
        
        this.add(p1);
        setSize(300, 200);
        setVisible(true);
    }
    
    public static void main(String[] args) {
    	Test frame = new Test();
    	
    }
}

```
![image](https://user-images.githubusercontent.com/54658614/223477273-d219a48f-0423-4277-bd0a-0db1127fbde6.png)

### JLabel
- JLabel 클래스는 정보 또는 텍스트를 위한 라벨을 생성한다.
- JLabel 클래스는 문자열이나 아이콘을 사용하여 객체를 생성한다.

#### JLabel 클래스와 생성자
|생성자|설명|
|---|---|
|JLabel()|text와 이미지를 사용하지 않는 JLabel을 생성한다.|
|JLabel(Icon image)|image를 Icon으로 사용하는 JLabel을 생성한다.|
|JLabel(Icon image, int horizontalAlignment)|image를 Icon으로 사용하고, horizontalAlignment의 값에 따라 정렬하는 JLabel을 생성한다.|
|JLabel(String text)|text를 사용하는 JLabel을 생성한다.|
|JLabel(String text, Icon icon, int horizontalAlignment)|text와 icon을 사용하고, horizontalAlignment의 값에 따라 정렬하는 JLabel을 생성한다.|
|JLabel(String text, int horizontalAlignment)|text를 사용하고, horizontalAlignment의 값에 따라 정렬하는 JLabel을 생성한다.|

### JTextField
JTextField 클래스는 한 줄의 문자열을 입력할 수 있는 컴포넌트이다.

#### JTextField 클래스의 생성자
|생성자|설명|
|---|---|
|JTextField()|초기 문자열이 null이고 길이가 0인 텍스트 필드를 생성한다.|
|JTextField(String text)|초기 문자열이 text이고 길이가 0인 텍스트 필드를 생성한다.|
|JTextField(int column)|초기 문자령리 null이고 길이가 columns인 텍스트 필드를 생성한다.|
|JTextField(String text, int columns)|초기 문자열이 text이고 길이가 columns인 텍스트 필드를 생성한다.|

#### JTextField의 주요 메서드
|메서드|설명|
|---|---|
|String getText()|텍스트 필드에 입력된 문자열을 구한다.|
|void setText(String text)|지정된 문자열을 텍스트 필드에 쓴다.|
|void setEditable(boolean)|텍스트를 입력할 수 있는지 없는지 설정한다.|
|boolean isEditable()|텍스트를 입력할 수 있는지 없는지 반환한다.|

### JTextArea
- JTextArea 클래스는 여러 줄의 문자열을 입력할 수 있는 컴포넌트이다.
- JTextArea는 창의 크기보다 많은 문자열을 입력하더라도 자동으로 스크롤바가 생기지 않는다.
- 따라서 스크롤바의 기능을 사용하기 위해서는 JScrollPane 클래스를 사용해서 표시해야 한다.

#### JTextArea 클래스의 생성자
|생성자|설명|
|-----|------|
|JTextArea()|초기 문자열이 null이고 행과 열이 각각 0인 텍스트에어리어를 생성한다.|
|JTextArea(String text)|초기 문자열이 text이고 행과 열이 각각 0인 텍스트에어리어를 생성한다.|
|JTextArea(int rows, int columns)|초기 문자열이 null이고 행이 rows이고 열이 columns인 텍스트에어리어를 생성한다.|
|JTextArea(String text, int rows, int columns)|초기 문자열이 text이고 행이 rows이고 열이 columns인 텍스트에어리어를 생성한다.|

#### JPasswordFIeld 
- JPasswordField  클래스는 비밀번호와 같이 입력받은 글자를 보여주지 않아야 할 때 사용하는 컴포넌트이다.

|생성자|설명|
|-----|------|
|JPasswordField()|JPasswordFIeld를 생성한다.|
|JPasswordField(String text)|초기 문자열이 text인 패스워드인 필드를 생성한다.|
|JPasswordFIeld(int columns)|열의 수(길이)가 columns인 패스워드 필드를 생성한다.|
|JPasswordField(String text, int columns)|초기 문자열이 text이고, 열의 수가 columns인 패스워드 필드를 생성한다.|

ex2_component 패키지 생성
#### Ex1_JText클래스 생성
```java
package test;

import java.awt.FlowLayout;

import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JPasswordField;
import javax.swing.JTextArea;
import javax.swing.JTextField;

public class Test {
	public static void main(String[] args) {
		JFrame f = new JFrame();
		f.setLayout(new FlowLayout());
		JLabel lb1 = new JLabel("이름");
		JLabel lb2 = new JLabel("나이");
		JLabel lb3 = new JLabel("비밀번호");
		
		JTextField tf = new JTextField(20);
		JTextArea ta = new JTextArea(7,20);
		JPasswordField pf = new JPasswordField(20);
		
		f.add(lb1);
		f.add(tf);
		f.add(lb2);
		f.add(ta);
		f.add(lb3);
		f.add(pf);
		
		f.setBounds(400,400,305,250);
		f.setVisible(true);
		f.setDefaultCloseOperation(f.EXIT_ON_CLOSE);
	}
}

```
![image](https://user-images.githubusercontent.com/54658614/223490882-f5da6467-f1b4-4ea5-a061-a2a401075466.png)

### JCheckBoxs
- JCheckBox 클래스는 체크 박스 기능을 제공하며, AbstractButton 클래스로부터 상속받는다. 

#### JCheckBox 클래스의 생성자

|생성자|설명|
|-----|------|
|JCheckBox()|글자와 아이콘을 사용하지 않고, 선택되지 않은 상태의 JCheckBox를 생성한다.|
|JCheckBox(Icon icon)|아이콘을 사용하고, 선택되지 않은 상태의 JCheckBox를 생성한다.|
|JCheckBox(Icon icon, boolean selected)|아이콘을 사용하는 JCheckBox를 생성한다. selected가 true이면 체크박스가 선택된 상태로 나나나며, false이면 선택되지 않은 상태로 나타난다.|
|JCheckBox(String text)|글자를 사용하는 선택되지 않은 상태의 JCheckBox를 생성한다.|
|JCheckBox(String text, boolean selected)|글자와 아이콘을 사용하는 선택되지 않은 상태의 JCheckBox를 생성한다.|
|JCheckBox(String text, Icon icon, boolean selected)|글자와 아이콘을 사용하는 JCheckBox를 생성한다. selected가 true이면 체크박스가 선택된 상태로 나타나며, false이면 선택되지 않은 상태로 나타난다.|

- 체크 박스의 상태는 다음의 메서드를 이용하여 설정할 수가 있으며, selected를 true로 설정하면 체크 박스가 선택된 상태로 나타난다.

### JRadioButton
- JRadioButton 클래스는 라디오 버튼 기능을 제공하며, AbstractButton 클래스로부터 상속받는다. 

#### JRadioButton 클래스의 생성자

|생성자|설명|
|-----|------|
|JRadioButton()|글자가 없고 선택되지 않은 상태의 JRadioButton을 생성한다.|
|JRadioButton(Icon icon)|아이콘을 사용하고 선택되지 않은 상태의 JRadioButton을 생성한다.|
|JRadioButton(Icon icon, boolean selected)|아이콘을 사용하는 JRadioButton을 생성한다. selected가 true이면 체크박스가 선택된 상태로 나타나며, false이면 선택되지 않은 상태로 나타난다.|
|JRadioButton(String text)|글자를 사용하는 선택되지 않은 상태의 JRadioButton을 생성한다.|
|JRadioButton(String text, boolean selected)|글자를 사용하는 JRadioButton을 생성한다. selected가 true이면 체크박스가 선택된 상태로 나타나며, false이면 선택되지 않은 상태로 나타난다.|
|JRadioButton(String text, Icon icon)|글자의 아이콘을 사용하는 선택되지 않은 상태의  JRadioButton을 생성한다.|
|JRadioButton(String text, Icon icon, boolean selected)|글자와 아이콘을 사용하는 JRadioButton을 생성한다. selected가 true이면 체크박스가 선택된 상태로 나타나며, false이면 선택되지 않은 상태로 나타난다.|

- 여러개의 라디오 버튼은 ButtonGroup을 사용하여 하나의(논리적) 그룹으로 묶을 수가 있다. 그룹으로 묶으면 여러 개의 라디오 버튼에서 하나만이 선택되어진다. 라디오 버튼을 그룹으로 묶으면 ButtonGroup 클래스에서 제공하는 add()메서드를 사용한다.

#### Ex3_JCheckBoxTest 클래스 생성
```java
package test;

import javax.swing.ButtonGroup;
import javax.swing.JCheckBox;
import javax.swing.JFrame;
import javax.swing.JPanel;
import javax.swing.JRadioButton;

public class JCheckBoxTest extends JFrame {

	JCheckBox jcb1, jcb2, jcb3;
	JRadioButton jrb1, jrb2, jrb3, jrb4, jrb5;
	JPanel jp1, jp2, jp3;
	
	JCheckBoxTest() {
		super("체크박스와 라디오 버튼 만들기");
		
		// 체크 박스 등록 
		jp1 = new JPanel();
		jcb1 = new JCheckBox("음악감상", true);
		jcb2 = new JCheckBox("등산", true);
		jcb3 = new JCheckBox("조깅", false);
		jp1.add(jcb1);
		jp1.add(jcb2);
		jp1.add(jcb3);
		
		add(jp1, "North");
		
		// 결혼여부 라디오 버튼 등록
		jp2 = new JPanel();
		jrb1 = new JRadioButton("결혼", true);
		jrb2 = new JRadioButton("미혼", false);
		ButtonGroup bg1 = new ButtonGroup();
		bg1.add(jrb1);
		bg1.add(jrb2);
		
		jp2.add(jrb1);
		jp2.add(jrb2);
		add(jp2, "Center");
		
		// 주거형 라디오 버튼 등록
		jp3 = new JPanel();
		jrb3 = new JRadioButton("자가", true);
		jrb4 = new JRadioButton("전세", false);
		jrb5 = new JRadioButton("월세", false);
		
		ButtonGroup bg2 = new ButtonGroup();
		bg2.add(jrb3); 
		bg2.add(jrb4);
		bg2.add(jrb5);
		
		jp3.add(jrb3);
		jp3.add(jrb4);
		jp3.add(jrb5);
		add(jp3, "South");
		
		setSize(300, 200);
		setVisible(true);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
	}
	
	public static void main(String[] args) {
		new JCheckBoxTest();
	}
}

```
![image](https://user-images.githubusercontent.com/54658614/223492795-c68b54e2-f079-4320-b675-8b5fc138fa16.png)

### ItemEvent
- ItemEvent는 체크박스, 라디오버튼, 리스트의 항목, 메뉴의 항목이 선택되었거나 해제될 때에 발생한다.

#### ItemEvent 생성자

|생성자|설명|
|----|------|
|ItemEvent(ItemSelectable src, int type, Object entry, int state)|src는 이벤트를 발생시킨 컴포넌트, type은 이벤트 유형, entry는 이벤트 발생 시에 전달하고자 하는 특수한 아이템 객체, static은 아이템의 현재 상태를 의미한다.|

- ItemEvent는 이벤트 유형을 구분하기 위한 두 개의 상수를 제공한다.

#### 이벤트 유형 상수

|이벤트 유형|설명|
|----|------|
|SELECTED|한 항목이 선택되었을 때의 상수|
|DESELECTED|선택된 항목이 해제되었을 때의 상수|

#### ItemEvent의 메서드

|메서드|설명|
|----|------|
|Object getItem()|이벤트를 발생시킨 객체를 반환한다.|
|ItemSelectable getItemSelectable()|이벤트를 발생시킨 ItemSelectable 객체를 반환한다. 선택 박스나 리스트 등은 ItemSelectable 인터페이스를 이용하여 구현한다.|
|int getStateChange()|이벤트의 발생으로 변환된 상태를 상수로 반환한다.|

- ItemEvent는 ItemListener 인터페이스를 구현(implements)하여 사용하는데, ItemListener가 가지고 있는 메서드인 itemStateChanged(ItemEvent e)를 반드시 오버라이딩 해 주어야 한다.

```java
package test;

import java.awt.BorderLayout;
import java.awt.FlowLayout;
import java.awt.event.ItemEvent;
import java.awt.event.ItemListener;

import javax.swing.ButtonGroup;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.JRadioButton;

public class Test{
	public static void main(String[] args) {
		JFrame f = new JFrame("ItemEvent 처리");
		f.setLayout(new BorderLayout());
		
		//라디오버튼 객체 생성하기
		ButtonGroup rgroup = new ButtonGroup();
		JRadioButton r1 = new JRadioButton("선택1");
		JRadioButton r2 = new JRadioButton("선택2");
		JRadioButton r3 = new JRadioButton("선택3");
		
		//그룹에 라디오버튼 추가하기
		rgroup.add(r1);
		rgroup.add(r2);
		rgroup.add(r3);
		
		//패널에 라디오 버튼 붙히기
		JPanel jp1 = new JPanel();
		jp1.setLayout(new FlowLayout());
		jp1.add(r1);
		jp1.add(r2);
		jp1.add(r3);
		
		//패널 프레임의 가운데 붙히기
		f.add(jp1, BorderLayout.CENTER);
		
		JPanel jp2 = new JPanel(new FlowLayout());
		JLabel txt1 = new JLabel("선택 항목 : ");
		JLabel txt2 = new JLabel();
		jp2.add(txt1);
		jp2.add(txt2);
		f.add(jp2, BorderLayout.SOUTH);
		
		//라디오버튼의 이벤트를 담당할 메서드 재정의하기
		ItemListener il = new ItemListener() {
			
			@Override
			public void itemStateChanged(ItemEvent e) {
				if (e.getStateChange() == ItemEvent.SELECTED) {
					if (e.getSource() == r1) {
						txt2.setText("선택1");
					} else if (e.getSource() == r2 ) {
						txt2.setText("선택2");
					} else if (e.getSource() == r3) {
						txt2.setText("선택3");
					}
				}
				
			}
		};
		
		//라디오버튼에 만들어준 기능 붙히기
		r1.addItemListener(il);
		r2.addItemListener(il);
		r3.addItemListener(il);
		
		
		f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		f.setSize(300, 200);
		f.setVisible(true);	
	}
}

```

### JComboBox

- JComboBox 클래스는 텍스트 필드와 풀다운 리스트를 조합한 형태의 콤보 박스의 기능을 제공한다. 콩보 박스는 텍스트 필드에 하나의 항목만 나타내지만, 마우스로 항목을 선택하면 풀다운 형태의 리스트를 제공한다.

#### JComboBox 클래스의 생성자

|생성자|설명|
|-----|------|
|JComboBox()|아이템이 없는 콤보박스를 생성한다.|
|JComboBox(Vector items)|특정한 Vector로 부터 아이템을 취하는(콤보 박스를 초기화시키는) 콤보박스를 생성한다.|
|JComboBox(Object[] items)|배열 items로부터 아이템을 취하는 콤보박스를 생성한다.|


#### JComboBox 메서드

|메서드|설명|
|-----|------|
|void setSelectedItem(Object anObject)|anObject가 리스트에 있으면 그 아이템을 선택하고 없으면 리스트의 첫 번째 아이템을 선택한다.|
|Object getSelectedItem()|선택된 아이템을 구한다.|
|void setSelectedIndex(int anIndex)|anIndex에 위치한 아이템을 선택한다.|
|int getSelectedIndex()|선택된 아이템의 인덱스를 구한다. 선택된 아이템이 없거나 사용자가 필드에 리스트에 없는 값을 입력했을 경우 -1을 리턴한다.|
|void addItem(Object anObject)|콤보박스의 아이템 리스트에 항목인 anObject를 추가한다. 이 메서드는 JComboBox가 기본 데이터 모델을 사용할 때에만 작동한다.|
|void InsertItemAt(Object anObject, int index)|주어진 인덱스에 아이템을 삽입한다. 이 메소드는 JComboBox가 기본 데이터 모델을 사용할 때에만 작동한다.|
|void removeItem(Object anObject)|아이템 리스트에서 아이템을 삭제한다. 이 메소드는 JComboBox가 기본 데이터 모델을 사용할 때에만 작동한다.|
|void removeItem(int anIndex)|anIndex에 위치한 아이템을 삭제한다. 이 메모스든 JCheckBox가 기본 데이터 모델을 사용할 때에만 작동한다.|
|void removeAllItems()|모든 아이템을 삭제한다. 이 메서드는 JComboBox가 기본 데이터 모델을 사용할 때에만 작동한다.|

#### Ex4_JComboBoxTest 클래스 생성
```java
package test;

import javax.swing.JComboBox;
import javax.swing.JFrame;

public class Test{
	public static void main(String[] args) {
		JFrame f = new JFrame("콤보박스만들기");
		f.setLayout(null);
		String[] title = {"C", "비주얼베이직", "JAVA 프로그래밍", "자료구조", "이산수학"};
		JComboBox<String> jcm1 = new JComboBox<String>(title);
		
		jcm1.setBounds(50,50,100,30);
		f.add(jcm1);
		
		f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		f.setBounds(300,200,300,200);
		f.setVisible(true);

	}

}
```

![image](https://user-images.githubusercontent.com/54658614/225817183-0edf1699-ac10-4c47-ad66-1f84cd5a6b18.png)


### JScollPane
- JScollPane 클래스는 스크롤바의 기능을 제공한다.
- 보편적으로 JScrollPane 클래스는 패널(JPanel) 클래스에 스크롤바를 설정하는데 사용된다.
- 필요에 따라 패널에 수평이나 수직 스크롤바를 설정한다.

#### JScrollPane 클래스의 생성자

|생성자|설명|
|-----|------|
|JScrollPane()|수직, 수평 스크롤바가 필요할 때 표시되는 스크롤판을 생성한다.|
|JScollPane(Component view)|스크롤바가 추가될 컴포넌트 view에 스크롤판을 생성한다.|
|JScrollPane(Component view, int vsb, in hsb)|스크롤바가 추가될 컴포넌트 view에 스크롤판을 생성한다. 수직(vsb), 수평(hsb) 스크롤바를 설정하기 위한 상수가 올수 있다.|
|JScrollPane(int vsb, int hsb)|vsb와 hsb의 값에 따라 수직, 수평 스크롤바를 표시하는 스크롤판을 생성한다.|


#### vsb와 hsb의 상수 값

<table>
<thead>
	<tr>
		<th>구분</th>
		<th>상수</th>
		<th>설명</th>
	</tr>
</thead>
<tbody>
	<tr>
		<td rowspan='3'>vsb(수직)</td>
		<td>VERTICAL_SCROLLBAR_ALWAYS</td>
		<td>항상 수직 스크롤바를 표시한다.</td>
	</tr>
	<tr>
		<td>VERTICAL_SCROLLBAR_AS_NEEDED</td>
		<td>필요한 때만 수직 스크롤바를 표시한다.|
	</tr>
	<tr>
		<td>VERTICAL_SCROLLBAR_NEVER</td>
		<td>수직 스크롤바를 표기하지 않는다.</td>
	</tr>
	<tr>
		<td rowspan='3'>hsb(수평)</td>
		<td>HORIZONTAL_SCROLLBAR_ALWAYS</td>
		<td>항상 수평 스크롤바를 표시한다.</td>
	</tr>
	<tr>
		<td>HORIZONTAL_SCROLLBAR_AS_NEEDED</td>
		<td>필요할 때만 수평 스크롤바를 표시한다.</td>
	</tr>
	<tr>
		<td>HORIZONTAL_SCROLLBAR_NEVER</td>
		<td>수평 스크롤바를 표시하지 않는다.</td>
	</tr>
</tbody>
</table>

#### Ex4_JScrollPaneTest 클래스 생성
```java
package test;

import java.awt.BorderLayout;
import java.awt.GridLayout;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.ScrollPaneConstants;

public class Test{
	public static void main(String[] args) {
		JFrame f = new JFrame("");
		f.setLayout(new BorderLayout());
		
		JPanel jp = new JPanel();
		jp.setLayout(new GridLayout(10,5));
		int cnt = 1;
		for(int i = 1; i<=10; i++) {
			for(int j = 1; j<=5; j++) {
				jp.add(new JButton("버튼" + cnt));
			}
		}
		
		//수직, 수평 스크롤바 설정하기 위한 상수
		int v = ScrollPaneConstants.VERTICAL_SCROLLBAR_ALWAYS;
		int h = ScrollPaneConstants.HORIZONTAL_SCROLLBAR_ALWAYS;
		JScrollPane js = new JScrollPane(jp,v,h);
		
		f.add(js, BorderLayout.CENTER);
		
		
		f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		f.setBounds(300,200,300,200);
		f.setVisible(true);
	}

}

```
![image](https://user-images.githubusercontent.com/54658614/223494533-a745d419-6d34-460a-8e58-bbcb165f48cd.png)


### JTable
- JTable 클래스는 데이터를 테이블 형태인 행과 열로 나타내고자 할 때 사용한다.
- JTable 클래스로 나타낸 테이블에서 행은 마우스를 이용하여 경계선을 조정하고 위치를 바꿀 수 있다.

#### JTable 클래스와 생성자

|생성자|설명|
|-----|------|
|JTable()|테이블을 생성한다.|
|JTable(int numRows, int numColumns)|행의 수가 numRows이고 열의 수가 numColumns인 테이블을 생성한다.|
|JTable(Object[][] rowData, Object[] columnNames)|테이블에 나타낼 2차원 배열인 rowData와 행의 제목을 나타내는 일차원 배열인 columnNames을 가지고 테이블을 생성한다.|

- 테이블의 각 행에 들어갈 데이터인 이차원 배열 객체를 생성한다.
- 테이블(JTable) 객체 생성한다.
- JScrollPane에 테이블을 붙인다.

#### Ex5_JTableTest 클래스 생성
```java
package test;

import java.awt.BorderLayout;

import javax.swing.JFrame;
import javax.swing.JScrollPane;
import javax.swing.JTable;
import javax.swing.ScrollPaneConstants;

public class Test{
	public static void main(String[] args) {
		JFrame f = new JFrame("테이블만들기");
		f.setLayout(new BorderLayout());
		
		String[] title = {"사번", "성명", "부서"};
		String[][] data = {{"1", "고애신", "총무과"}, {"2", "최유신", "인사과"}, {"3", "구동매", "전산과"}};
		JTable table = new JTable(data, title);
		
		//수직, 수평 스크롤바 설정하기 위한 상수
		int v = ScrollPaneConstants.VERTICAL_SCROLLBAR_AS_NEEDED;
		int h = ScrollPaneConstants.HORIZONTAL_SCROLLBAR_AS_NEEDED;
		
		JScrollPane js = new JScrollPane(table,v,h);
		
		f.add(js, BorderLayout.CENTER);
		
		f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		f.setBounds(300,200,300,200);
		f.setVisible(true);
	}

}


```
![image](https://user-images.githubusercontent.com/54658614/223495069-9f9f3539-cb58-4c08-b9e5-87cc9a20a71d.png)


### 메뉴 - JMenuBar, JMenu, JMenuItem
- 스윙에서 메뉴 관련 클래스는 JMenuBar, JMenu, JMenuItem, JCheckBoxMenuItem, JRadioButtonMenuItem이 있다.
- 메뉴는 setJMenuBar() 메서드를 제공하는 컨테이너의 객체에만 사용할 수 있다. 이러한 컨테이너는 최종 컨테이너로 사용되는 JFrame 컨테이너만 가능하다.

#### JMenuBar 클래스의 생성자

|생성자|설명|
|-----|------|
|JMenuBar()|메뉴바를 생성한다.|

#### JMenu 클래스의 생성자

|생성자|설명|
|-----|------|
|JMenu()|레이블인 문자열이 없는 메뉴를 생성한다.|
|JMenu(String s)|레이블로 문자열 s를 사용하는 메뉴를 생성한다.|
|JMenu(String s, boolean b)|레이블로 문자열 s를 사용하는 메뉴를 생성한다. b의 값이 true인 경우 메뉴를 분리할 수 있다.|

#### JMenuItem 클래스의 생성자

|생성자|설명|
|-----|------|
|JMenuItem()|메뉴의 레이블로 문자열이나 아이콘이 없는 메뉴 아이템을 생성한다.|
|JMenuItem(Icon icon)|메뉴의 레이블로 아이콘을 사용하는 메뉴아이템을 생성한다.|
|JMenuItem(String text)|메뉴의 레이블로 문자열을 사용하는 메뉴아이템을 생성한다.|
|JMenuItem(String text, Icon icon)|메뉴의 레이블로 문자열과 아이콘을 사용하는 메뉴아이템을 생성한다.|
|JMenuItem(String text, int mnemonic)|메뉴의 레이블로 문자열과 키보드 mnemonic(단축키)를 갖는 메뉴아이템을 생성한다.|

#### Ex6_JMenuTest 클래스 생성
```java
package test;

import javax.swing.JFrame;
import javax.swing.JMenu;
import javax.swing.JMenuBar;
import javax.swing.JMenuItem;

public class Test{
	public static void main(String[] args) {
		JFrame f = new JFrame("JMenuTest");
		
		JMenuBar jmb = new JMenuBar();
		JMenu jmu1 = new JMenu("파일");
		JMenu jmu2 = new JMenu("편집");
		JMenu jmu3 = new JMenu("보기");
		
		JMenuItem jmi1 = new JMenuItem("새로만들기");
		JMenuItem jmi2 = new JMenuItem("열기");
		JMenuItem jmi3 = new JMenuItem("저장");
		
		jmu1.add(jmi1);
		jmu1.add(jmi2);
		jmu1.add(jmi3);
		
		JMenuItem jmi4 = new JMenuItem("잘라내기");
		JMenuItem jmi5 = new JMenuItem("복사");
		JMenuItem jmi6 = new JMenuItem("붙여넣기");
		
		jmu2.add(jmi4);
		jmu2.add(jmi5);
		jmu2.add(jmi6);
		
		JMenuItem jmi7 = new JMenuItem("도구모음");
		JMenuItem jmi8 = new JMenuItem("상태표시줄");
		
		jmu3.add(jmi7);
		jmu3.add(jmi8);
		
		jmb.add(jmu1);
		jmb.add(jmu2);
		jmb.add(jmu3);
		
		f.setJMenuBar(jmb);
		
		f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		f.setBounds(300,200,300,200);
		f.setVisible(true);
	}
}

```
![image](https://user-images.githubusercontent.com/54658614/225821525-44000a71-22d0-4f4c-ba94-d82578a8db8a.png)


### JPopupMenu
- JPopupMenu 클래스는 팝업 메뉴 기능을 제공한다.
- 일반적으로 팝업 메뉴는 마우스의 오른쪽 버튼을 누르거나(mousePressed), 해제(mouseReleased)할 때 수행한다.

|생성자|설명|
|-----|------|
|JPopupMenu()|팝업 메뉴를 생성한다.|
|JPopupMenu(String label)|기술된 텍스트를 팝업 메뉴의 레이블로 사용하는 팝업메뉴를 생성한다.|

#### Ex7_JPopupMenuTest 클래스 생성
```java
package test;

import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;

import javax.swing.ButtonGroup;
import javax.swing.JFrame;
import javax.swing.JPopupMenu;
import javax.swing.JRadioButtonMenuItem;

public class Test{
	public static void main(String[] args) {
		JFrame f = new JFrame("팝업메뉴에서 항목 선택");
		
		String[] title = {"사번", "성명", "부서" };
		JRadioButtonMenuItem[] rbm = new JRadioButtonMenuItem[3];
		
		JPopupMenu pmenu = new JPopupMenu();
		ButtonGroup tgroup = new ButtonGroup();
		
		for (int i = 0; i < rbm.length; i++) {
			rbm[i] = new JRadioButtonMenuItem(title[i]);
			pmenu.add(rbm[i]);
			tgroup.add(rbm[i]);
		}
		
		// 마우스 이벤트를 리스너에 등록
		f.addMouseListener(new MouseAdapter() {
			public void mousePressed(MouseEvent e) {
				checkForTriggerEvent(e);
			}
			
			public void mouseReleased(MouseEvent e) {
				checkForTriggerEvent(e);
			}
			
			// 마우스 오른쪽 버튼을 누르거나 해제할 때 발생
			private void checkForTriggerEvent(MouseEvent e) {
				if (e.isPopupTrigger()) 
					pmenu.show(e.getComponent(), e.getX(), e.getY());
			}
		});
		
		f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		f.setBounds(300,200,300,200);
		f.setVisible(true);
	}
}

```
![image](https://user-images.githubusercontent.com/54658614/223496283-caa61cb0-9d39-434f-82fd-e22c08be236d.png)

## 어댑터를 이용한 이벤트 처리
- 리스너 인터페이스를 구현(implements)하기 위해서는 반드시 재정의 해 주어야 한다. 
- 만약에 MouseListener를 구현해야 하는 경우에, 필요가 없더라도 이 인터페이스가 가지고 있는 5개의 메서드를 모두 재정의 해 주어야 한다.
- 이러한 문제를 해결해 주는 방법이 <b>어댑터 클래스(Adapter Class)</b>이다.
- 어댑터 클래스는 리스너가 가지고 있는 메서드 중에서 필요한 메서드만 재정의 해주면 된다.
- 필요한 경우에 일부 메서드만 어댑터 클래스를 상속 받는 것이 리스너 인터페이스를 구현하는 것 보다 더 편리하다.
- 어댑터 클래스는 인터페이스의 기능을 그대로 옮겨놓은 추상 클래스이다.

#### 리스너 인터페이스와 관련된 어댑터 클래스

|리스너 인터페이스|어댑터 클래스|
|----|------|
|ActionListener|없음|
|ChangeListener|없음|
|ItemListener|없음|
|KeyListener|KeyAdapter|
|ListSelectionListener|없음|
|MouseListener|MouseAdapter, MouseMotionAdapter|
|WindowListener|WindowAdapter|

- 자바에서의 어댑터는 클래스이기 때문에 앞의 리스너 인터페이스처럼 구현으로 사용할 수가 없고, 내부 클래스를 이용하여 사용해야 한다.

### ChangeEvent와 JSlider
- ChangeEvent는 JSlider와 같은 컴포넌트에서 값이 변경될 때에 발생된다.
- ChangeEvent는 ChangeListener 인터페이스를 구현하여 사용하는데, ChangeListener가 가지고 있는 메서드인 stateChanged(ChangeEvent e)를 재정의 해 주어야 한다.

- ChangeEvent는 JSlider의 값이 변경될 때마다 발생된다. 슬라이더 손잡이를 움직이거나 애플리케이션에서 JSlider의 setValue() 메서드를 호출하여 value값을 변경할 때 ChangeEvent가 발생한다.

#### day23/JSliderChangeEvent.java
```
package day23;
import javax.swing.*;
import java.awt.*;
import javax.swing.event.*;
public class JSliderChangeEvent extends JFrame {
	
	JLabel colorLabel;
	JSlider jsl = new JSlider();
	JSliderChangeEvent() {
		setTitle("슬라이더와 ChangeEvent");
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setLayout(new FlowLayout());
		colorLabel = new JLabel("    SLIDER EXAMPLE      ");
		//0~100 사이의 값을 선택할 수 있는 슬라이더 생성. 초기값은 50
		jsl = new JSlider(JSlider.HORIZONTAL,0, 255, 50);
		jsl.setPaintLabels(true);
		jsl.setPaintTicks(true);
		jsl.setPaintTrack(true);
		jsl.setMajorTickSpacing(50);
		jsl.setMinorTickSpacing(10);
		
		jsl.addChangeListener(new MyChangeListener());
		add(jsl);
		
		jsl.setForeground(Color.RED);
		colorLabel.setOpaque(true);
		
		colorLabel.setBackground(new Color(0, jsl.getValue(), 0));
		add(colorLabel);
		setSize(300, 300);
		setVisible(true);
	}
	
	
	class MyChangeListener implements ChangeListener {
		// 슬라이더 컴포넌트의 값이 변경되면 호출됨
		public void stateChanged(ChangeEvent e) {
			colorLabel.setBackground(new Color(0, jsl.getValue(), 0));
		}
	}
	
	public static void main(String[] args) {
		new JSliderChangeEvent();
	}
}
```

### Image 넣기

ex2_img 패키지 생성

src에서 폴더생성 images 구글에서 심슨 이미지 검색<br>
웹개발 폴더에 저장 복사해서 images 폴더에 저장 작은크기의 이미지도 같은 방법으로<br>
버튼을 눌렀을 때 바뀔 이미지를 한 장 더 구한다.<br>

#### ImgText 클래스 생성
```java
public class ImgText {
	static boolean change = true;

	public static void main(String[] args) {
		Frame f = new Frame(“이미지”);
		f.setBounds(500,300,500,500);
		f.setLayout(null);
		이미지는 프레임에 맞게 늘릴수 없고 꽉 채우고 싶으면 맞는 사진을 구해야 함

		스윙이라고 해서 좀 더 발전된 클래스에서 제공하는 기능

		ImageIcon back = new ImageIcon(“src/images/s.png”);
		JLabel jl_back = new JLabel(back);
		jl_back.setBounds(0,0,500,500);

		ImageIcon btnIcon = new ImageIcon(“src/images/ic.png”);
		JButton jb = new JButton(btnIcon);
		jb.setBounds(20,40,106,106);

		ImageIcon back2 = new ImageIcon(“src/images/s2.jpg”);
		
		jb.addActionListener(new ActionListener() {
			@Override
			public void actionPerformed(ActionEvent e) {
				if(change == true){
					//배경으로 사용중인 JLabel의 아이콘을 back2로 변경
					jl_back.setIcon(back2);
					f.repaint();//현재 프레임의 내용을 갱신
				} else if(change == false){
					//배경으로 사용중인 JLabel의 아이콘을 backㅎ로 변경
					jl_back.setIcon(back);
					f.repaint();//현재 프레임의 내용을 갱신
				}
				
			}
		});

		//버튼객체인 jb를 먼저 add해줘야 배경보다 위쪽에 보여진다.
		f.add(jb);
		f.add(jl_back);
		

		frame.setVisible(true);

		f.addWindowListener(new WindowAdapter() {
			@Override
			public void windowClosing(WindowEvent e) {
				System.exit(0);
			}
		});
		
	}
}
```

## 메모장 만들기

memo 패키지 만들기

#### MemoMain 클래스
```java
package test;

import java.awt.Color;
import java.awt.FileDialog;
import java.awt.Font;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.FileWriter;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JPanel;
import javax.swing.JTextArea;
import javax.swing.JTextField;

public class Memo {
	public static void main(String[] args) {
		JFrame frame = new JFrame("메모장");
		JPanel jp = new JPanel();
		jp.setBackground(Color.CYAN);
		jp.setLayout(null);

		Font font = new Font("", Font.PLAIN, 20);

		JTextField tf = new JTextField();
		tf.setBounds(10, 15, 180, 30);
		tf.setFont(font);

		JButton btn_input = new JButton("확인");
		btn_input.setBounds(190, 15, 60, 30);
		btn_input.setEnabled(false);// 버튼 클릭 비활성화

		JTextArea ta = new JTextArea();
		ta.setBounds(10, 70, 230, 280);
		//ta.setEditable(false); // ta에 임의로 값을 추가할 수 없도록 하는 기능

		JButton btnSave = new JButton("저장");
		JButton btnClose = new JButton("종료");
		btnSave.setBounds(10, 356, 110, 30);
		btnClose.setBounds(130, 356, 110, 30);

		// tf에 값이 들어가 있는지 확인하여 ‘확인’버튼을 활성화 또는 비활성화
		tf.getDocument().addDocumentListener(new TextAdapter(tf,btn_input));
		// 확인버튼 클릭시 tf의 값을 ta로 복사해서 넣어주자!
		btn_input.addActionListener(new InputButtonAdapter(tf, ta));
		// 종료버튼에 이벤트감지자 등록
		btnClose.addActionListener(new ActionListener() {

			@Override
			public void actionPerformed(ActionEvent arg0) {
				// System.exit(0); 모든 프레임들을 종료
				frame.dispose(); // 현재 프레임만 종료
			}

		});

		// 저장 버튼을 눌렀을 때 ta의 값을 저장하는 이벤트 감지자 등록
		btnSave.addActionListener(new ActionListener() {
			@Override
			public void actionPerformed(ActionEvent e) {
				// ta에 쓰여진 내용을 가져오자
				String message = ta.getText();

				// 경로를 설정하는 FileDialog
				FileDialog fd = new FileDialog(frame, "저장", FileDialog.SAVE);
				// FileDialog.SAVE 오른쪽 아래 버튼이 저장으로 변경

				fd.setVisible(true);

				// fd를 통해 지정한 저장경로와 파일명을 알아내자
				// (fd.setVisible()보다 아래쪽에 작성할 것!!)
				String path = fd.getDirectory() + fd.getFile();
				System.out.println(path);

				// char기반의 스트림을 생성하여 path경로에 저장
				try {
					FileWriter fw = new FileWriter(path);
					fw.write(message);
					fw.close();
				} catch (Exception e1) {

				}
			}
		});

		jp.add(tf);
		jp.add(btn_input);
		jp.add(ta);
		jp.add(btnSave);
		jp.add(btnClose);
		frame.add(jp);
		
		frame.setBounds(700, 200, 270, 440);
	
		frame.setVisible(true);
		frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

	}

}

```

![image](https://user-images.githubusercontent.com/54658614/226518191-13b6882e-8e32-40ff-9aaf-39d82ec35581.png)


#### InputButtonAdapter 클래스 생성

```java
package test;

import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.TextEvent;
import java.awt.event.TextListener;

import javax.swing.JTextArea;
import javax.swing.JTextField;

public class InputButtonAdapter implements ActionListener {
	
	private JTextField tf;
	private JTextArea ta;

	public InputButtonAdapter(JTextField tf, JTextArea ta) {
		this.tf =tf;
		this.ta = ta;
	}

	@Override
	public void actionPerformed(ActionEvent e) {
		//ta내부의 모든 값을 변경하는 메서드 setText	
		//ta.setText(tf.getText());

		//ta가 가진 기존 값에 새로운 값을 이어붙이자(append)
		ta.append( tf.getText() + "\n");
		
		tf.requestFocus(); //tf로 커서를 이동시킨다.
		tf.setText("");
		
	}

}
```

#### TextAdapter 클래스 생성
```java
package test;

import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;

import javax.swing.JButton;
import javax.swing.JTextField;
import javax.swing.event.DocumentEvent;
import javax.swing.event.DocumentListener;

public class TextAdapter implements DocumentListener{


	private JTextField tf;
	private JButton btn_input;

	public TextAdapter(JTextField tf, JButton btn_input) {
		this.tf =tf ;
		this.btn_input = btn_input;
	}

	@Override
	public void changedUpdate(DocumentEvent arg0) {
		change();
		
	}

	@Override
	public void insertUpdate(DocumentEvent arg0) {
		change();
	}

	@Override
	public void removeUpdate(DocumentEvent arg0) {
		change();
	}
	
	public void change() {
		if(tf.getText().length() == 0) {
			btn_input.setEnabled(false);
		} else {
			btn_input.setEnabled(true);
		}
	}
}
```

## 계산기 만들기

![image](https://github.com/to7485/Java1900/assets/54658614/2e95d5ba-af30-4fe8-866f-578a2b5015d2)

```java
package test;

import java.awt.BorderLayout;
import java.awt.Button;
import java.awt.Color;
import java.awt.Dimension;
import java.awt.Font;
import java.awt.Frame;
import java.awt.GridLayout;
import java.awt.Label;
import java.awt.Panel;
import java.awt.Toolkit;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

//Button Calculator
public class Calculator extends Frame implements ActionListener {

	private static final long serialVersionUID = 1L;

	// 멤버 필드
	private String[] sustr = { "7", "8", "9", "4", "5", "6", "1", "2", "3", "0", "+/-", "." };
	private Button[] subt = new Button[sustr.length];
	private String[] funstr = { "BackSpace", "CE", "C" };
	private Button[] funbt = new Button[funstr.length];
	private String[] operstr = { "+", "-", "*", "/" };
	private Button[] operbt = new Button[operstr.length];
	private Button equbt = new Button("=");
	private Label disp = new Label("0.", Label.RIGHT);
	
	private boolean first = true; // true 처음, false 처음이 아님. -> 숫자 입력하기 위한 멤버필드
	private boolean jumcheck = false; //false면 소수점을 누르지  않음, true 소수점을 누름.  
	private char operator = '+'; //연산자
	private double result = 0;
	
	// 생성자
	public Calculator() {
		super("Calculator");
		buildGUI();
		setEvent();
	}

	// 메서드
	@Override
	public void actionPerformed(ActionEvent e) {
		
		// 숫자 입력 로직
		for(int i = 0; i<subt.length-2 ; i++) {
			if(e.getSource()== subt[i]) {
				
				//처음으로 눌렀음.
				if(first) {
					// 0 연속으로 누르는 것 방지
					if(disp.getText().equals("0.") && subt[i].getLabel().equals("0")) return;
					disp.setText(subt[i].getLabel()+".");
					first = false; //다음 번엔 처음이 아니게 한다.
				} 
				
				//처음이 아닌경우 
				else {
					String tmp = disp.getText();
					if(jumcheck) { //"." 을 누른 적이 있다.
						disp.setText(tmp + subt[i].getLabel());
					} else { // "."을 누른적이 없다. 
						tmp = tmp.substring(0, tmp.length()-1); //마지막 점을 뻄.
						disp.setText(tmp + subt[i].getLabel() + ".");
					}
				}
				return;
			}
		} //end subt
		
		
		// 부호 처리 로직
		if(e.getSource() == subt[subt.length-2]) {
			String tmp = disp.getText();
			if(tmp.equals("0.")) return; //처음 0일때  +/- 의미 x
			if(tmp.charAt(0) == '-') {
				disp.setText(tmp.substring(1));
			} else {
				disp.setText('-' + tmp);
			}
		}
		
		// 소수점 처리  로직 
		if(e.getSource()== subt[subt.length-1]) { 
			first = false;
			jumcheck = true;
			return;
		}
		
		// 연산자 처리  로직
		for(int i = 0 ; i<operbt.length ; i++) {
			if(e.getSource() == operbt[i]) {
				calc();//계산
				operator = operbt[i].getLabel().charAt(0);
				first = true;
				jumcheck = false;
			}
		}
		
		// '=' 처리로직 
		if(e.getSource() == equbt) {
			calc();
			first = true;
			jumcheck = false;
			operator = '+';
			result = 0;
			return;
		}
		
		// "backSpace" 처리로직 
		if(e.getSource() == funbt[0]) {
			String tmp = disp.getText(); 
			if(tmp.equals("0.")) return; 
			
			//2. 5. 인 경우..
			if(tmp.length() == 2) {
				disp.setText("0.");
				first = true;
				jumcheck = false;
				return;
			}
			//-4.등등 인경우.
			if(tmp.length() == 3 && tmp.charAt(0) =='-') {
				disp.setText("0.");
				first = true;
				jumcheck = false;
				return;
			}
			
			// + 인경우 ex)2.3
			if(tmp.length() == 3 && tmp.charAt(0) =='0') {
				disp.setText("0.");
				first = true;
				jumcheck = false;
				return;
			}
			
			if(tmp.charAt(tmp.length()-1)== '.') {
				tmp = tmp.substring(0,tmp.length() -2);
				disp.setText(tmp  +".");
			} else {
				disp.setText(tmp.substring(0,tmp.length() -1));
			}
			return;
		}
		
		// "CE" 처리로직 
		if(e.getSource() == funbt[1]) {
			disp.setText("0.");
			first = true;
			jumcheck = false;
			result = 0;
			return;		
		}
		
		// "C" 처리로직 
		if(e.getSource() == funbt[2]) {
			disp.setText("0.");
			first = true;
			jumcheck = false;
			operator = '+';
			result = 0;
			return;
		}
	}
	
	public void calc() {
		double value =Double.parseDouble(disp.getText());
		switch(operator){
			case '+' : result = result + value; break;
			case '-' : result = result - value; break;
			case '*' : result = result * value; break;
			case '/' : result = result / value; break;
		}
		
		//소수점 처리
		double val = result - (int)result;
		if(val > 0) {
			disp.setText(String.valueOf(result));
		} else {
			disp.setText(String.valueOf((int)result)+".");
		}
		
	}
	
	//이벤트 연결
	public void setEvent() {
		for(int i = 0 ; i<subt.length; i++) 
			subt[i].addActionListener(this);
		
		for(int i = 0 ; i<funbt.length; i++) 
			funbt[i].addActionListener(this);
		
		for(int i = 0 ;i<operbt.length; i++) 
			operbt[i].addActionListener(this);
		
		equbt.addActionListener(this);
		
		//익명클래스로 이벤트 달기
		addWindowListener(new WindowAdapter() {
			@Override
			public void windowClosing(WindowEvent e) {
				System.exit(0);
			}
		});
	}
	
	// 화면  구성
	public void buildGUI() {
		setBackground(Color.cyan);
		add("North", new Label()); //여백
		add("South", new Label()); //여백
		add("West", new Label()); //여백
		add("East", new Label()); //여백
		
		//기본 BorderLayout
		
		Panel mainPanel = new Panel(new BorderLayout(3,3));
			disp.setBackground(Color.white);
			disp.setFont(new Font("Serif", Font.BOLD,15));
			mainPanel.add("North", disp); 
			
			Panel cPanel = new Panel(new GridLayout(1,2,3,3));
				Panel cleftPanel = new Panel(new GridLayout(4,3,3,3));
					for(int i = 0 ;i<subt.length;i++) {
						subt[i] = new Button(sustr[i]); //버튼 초기화
						subt[i].setFont(new Font("Serif", Font.BOLD,15));
						cleftPanel.add(subt[i]);
					}
				cPanel.add(cleftPanel);
				
				Panel crightPanel = new Panel(new GridLayout(4,1,3,3));
					Panel cr1Panel =new Panel(new GridLayout(1,3,3,3));
						for(int i = 0 ; i<funbt.length; i++) {
							funbt[i] = new Button(funstr[i]);
							funbt[i].setFont(new Font("Serif", Font.BOLD,15));
							cr1Panel.add(funbt[i]);
						}
					crightPanel.add(cr1Panel);
					
					Panel cr2Panel =new Panel(new GridLayout(1,3,3,3));
					Panel cr3Panel =new Panel(new GridLayout(1,3,3,3));
						for(int i = 0 ; i<operstr.length ;i++) {
							operbt[i] = new Button(operstr[i]);
							operbt[i].setFont(new Font("Serif", Font.BOLD,15));
							if(i<2) cr2Panel.add(operbt[i]);
							else cr3Panel.add(operbt[i]);
						}
						
					crightPanel.add(cr2Panel);
					crightPanel.add(cr3Panel);
					
					equbt.setFont(new Font("Serif", Font.BOLD,15));
					crightPanel.add(equbt);
					
				cPanel.add(crightPanel);
			mainPanel.add("Center",cPanel);
		
		add("Center", mainPanel);
		
		pack();
		
		Dimension scr = Toolkit.getDefaultToolkit().getScreenSize();
		Dimension my = getSize();
		setLocation(scr.width / 2 - my.width / 2, scr.height / 2 - my.height / 2);
		
		setResizable(false);
		setVisible(true);
	}
	
	public static void main(String[] args) {
		new Calculator();
	}

}
```
