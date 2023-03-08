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

import javax.swing.*;
import java.awt.*;

public class Test extends JFrame {
	
	JTextField tf;
	JTextArea ta;
	JPasswordField pf;
	
	Test() {
		super("JLabelText");
		setLayout(new FlowLayout());
		setSize(300, 240);
		setLocation(400, 400);
		setVisible(true);
		setDefaultCloseOperation(EXIT_ON_CLOSE);
		
		JLabel lb1 = new JLabel("이름");
		JLabel lb2 = new JLabel("주소");
		JLabel lb3 = new JLabel("비밀");
		
		tf = new JTextField(20);
		ta = new JTextArea(7, 20);
		pf = new JPasswordField(20);
		
		add(lb1);
		add(tf);
		add(lb2);
		add(ta);
		add(lb3);
		add(pf);
	}
	
	public static void main(String[] args) {
		Test jlt = new Test();
	}
}

```
![image](https://user-images.githubusercontent.com/54658614/223490882-f5da6467-f1b4-4ea5-a061-a2a401075466.png)

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

import javax.swing.*;
import java.awt.*;

public class JComboBoxTest extends JFrame {
	JCheckBox jcb1, jcb2, jcb3;
	JComboBox<String> jcm1;
	JPanel jp1;
	String[] title = {"C", "비주얼베이직", "JAVA 프로그래밍", "자료구조", "이산수학"};
	JComboBoxTest() {
		super("콤보 박스 만들기");
		setLayout(new FlowLayout());
		jp1 = new JPanel();
		jcb1 = new JCheckBox("컴퓨터공학", true);
		jcb2 = new JCheckBox("전자상거래", true);
		jcb3 = new JCheckBox("경영학", false);
		jp1.add(jcb1);
		jp1.add(jcb2);
		jp1.add(jcb3);
		add(jp1);
		
		jcm1 = new JComboBox<>(title);
		add(jcm1);
		setSize(300, 250);
		setVisible(true);
		setDefaultCloseOperation(EXIT_ON_CLOSE);
	}
	
	public static void main(String[] args) {
		new JComboBoxTest();
	}
}

```

![image](https://user-images.githubusercontent.com/54658614/223493241-d7bf18ae-b23b-4c5a-88a7-ec7a4ac95d8e.png)


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

import javax.swing.*;
import java.awt.*;

public class JScrollPaneTest extends JFrame {
	JPanel jp;
	
	JScrollPaneTest() {
		super("JScrollPane Test");
		setSize(300, 300);
		setLayout(new BorderLayout());
		setLocation(300, 300);
		setVisible(true);
		
		jp = new JPanel();
		jp.setLayout(new GridLayout(10, 5));
		int cnt = 1;
		for (int i = 1; i <= 10; i++) {
			for (int j = 1; j <= 5; j++) {
				jp.add(new JButton("버튼" + cnt));
				cnt++;
			}
		}
		
		// 수직, 수평 스크롤바를 설정하기 위한 상수를 얻음
		int v = ScrollPaneConstants.VERTICAL_SCROLLBAR_ALWAYS;
		int h = ScrollPaneConstants.HORIZONTAL_SCROLLBAR_ALWAYS;
		JScrollPane  js = new JScrollPane(jp, v, h);
		add(js, BorderLayout.CENTER);
		
		setDefaultCloseOperation(EXIT_ON_CLOSE);
		
	}
	
	public static void main(String[] args) {
		new JScrollPaneTest();
		
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

import javax.swing.*;
import java.awt.*;

public class JTableTest extends JFrame {
	JTableTest() {
		super("JTable Test");
		setSize(300, 300);
		setLocation(300, 300);
		setLayout(new BorderLayout());
		String[] title = {"사번", "성명", "부서"};
		String[][] data = {{"1", "고애신", "총무과"}, {"2", "최유신", "인사과"}, {"3", "구동매", "전산과"}};
		JTable table = new JTable(data, title);
		int v = ScrollPaneConstants.VERTICAL_SCROLLBAR_AS_NEEDED;
		int h = ScrollPaneConstants.HORIZONTAL_SCROLLBAR_AS_NEEDED;
		
		JScrollPane js = new JScrollPane(table, v, h);
		add(js, BorderLayout.CENTER);
		
		setVisible(true);
		setDefaultCloseOperation(EXIT_ON_CLOSE);
	}
	
	public static void main(String[] args) {
		new JTableTest();
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

import javax.swing.*;
import java.awt.*;

public class JMenuTest extends JFrame {
	JMenuTest() {
		super("JMenuTest");
		setSize(300, 300);
		setLocation(300, 300);
		
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
		
		setJMenuBar(jmb);
		
		setVisible(true);
		setDefaultCloseOperation(EXIT_ON_CLOSE);
	}
	
	public static void main(String[] args) {
		new JMenuTest();
	}
}

```
![image](https://user-images.githubusercontent.com/54658614/223495526-06073cfe-7993-416f-9722-7cf2a4e24568.png)





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

import javax.swing.*;
import java.awt.*;
import java.awt.event.*;

public class JPopupMenuTest extends JFrame {
	
	
	JPopupMenuTest() {
		super("팝업메뉴에서 항목 선택");
		
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
		addMouseListener(new MouseAdapter() {
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
		
		setSize(300, 200);
		setVisible(true);
	}
	public static void main(String[] args) {
		new JPopupMenuTest();
	}
}

```
![image](https://user-images.githubusercontent.com/54658614/223496283-caa61cb0-9d39-434f-82fd-e22c08be236d.png)


### JTabbedPane
- JTabbedPane 클래스는 탭(Tab)의 기능을 제공한다.

|생성자|설명|
|-----|------|
|JTabbedPane()|비어있는 탭팬을 생성한다. 기본적인 탭의 위치는  JTabbedPane.TOP이 기본값이 된다.|
|JTabbedPane(int tabPlacement)|비어 있는 J탭팬을 생성한다. tabPlacement의 값에 따라 탭의 위치가 정해진다. tabPlacement의 값에는 JTabbedPane.TOP, JTabbedPane.BOTTOM, JTabbedPane.LEFT, JTabbedPane.RIGHT가 있다.|

- 탭(JTabbedPane)객체를 생성한다.
```
JTabbedPane jtp = new JTabbedPane(JTabbedPane.TOP);
```

- 탭에 addTab("탭의 레이블 이름", 추가할 객체 이름) 메소드를 사용하여 필요한 개수의 컴포넌트를 추가한다.

```
// 추가할 패널 객체 생성(만약 패널을 추가할 경우)
JPanel jpn1 = new JPanel();
JPanel jpn2 = new JPanel();
jtp.addTab("기본내용", jpn1);
jtp.addTab("기본내용", jpn2);
```

#### Ex8_JTabbedPaneFrame 클래스 생성
```java
package test;

import javax.swing.*;
import java.awt.*;

public class JTabbedPaneFrame extends JFrame {
	public JTabbedPaneFrame() {
		super("사원 개인정보 조회(JTabbedPane)");
		
		JTabbedPane jtp = new JTabbedPane(JTabbedPane.TOP);
		JPanel jpn1 = new JPanel();
		JPanel jpn2 = new JPanel();
		JPanel jpn3 = new JPanel();
		bTable jt1 = new bTable();
		eTable jt2 = new eTable();
		fTable jt3 = new fTable();
		
		jpn1.add(jt1);
		jpn2.add(jt2);
		jpn3.add(jt3);
		
		jtp.addTab("기본내용", jpn1);
		jtp.addTab("추가내용", jpn2);
		jtp.addTab("보안내용", jpn3);
		
		add(jtp, BorderLayout.CENTER);
		
		setSize(500, 200);
		setVisible(true);
	}
	
	public static void main(String[] args) {
		JTabbedPaneFrame jt = new JTabbedPaneFrame();
		jt.setDefaultCloseOperation(EXIT_ON_CLOSE);
	}
}


class bTable extends JPanel {
	public bTable() {
		String[] title = {"사번", "성명", "부서"};
		String[][] data = {{"1", "이름1", "총무과"}, {"2", "이승엽", "인사과"}, {"3", "박태환", "전산과"}};
		JTable table = new JTable(data, title);
		int v = ScrollPaneConstants.VERTICAL_SCROLLBAR_ALWAYS;
		int h = ScrollPaneConstants.HORIZONTAL_SCROLLBAR_ALWAYS;
		JScrollPane js = new JScrollPane(table, v, h);
		add(js);
	}
}

class eTable extends JPanel {
	public eTable() {
		String[] title = {"입사일", "주소", "전화" };
		String[][] data = {{"2001-1-1", "은평구 응암동", "303-5555"}, {"2000-5-30", "마토구 도화동", "5555-6666"}, {"2008-1-1", "구로구 신림동", "777-1234"}};
		JTable table = new JTable(data, title);
		int v = ScrollPaneConstants.VERTICAL_SCROLLBAR_ALWAYS;
		int h = ScrollPaneConstants.HORIZONTAL_SCROLLBAR_ALWAYS;
		JScrollPane js = new JScrollPane(table, v, h);
		add(js);
	}
}

class fTable extends JPanel {
	public fTable() {
		String[] title = {"호봉", "근무평점"};
		String[][] data = {{"0506", "보통"}, {"0401", "우수"}, {"0701", "미흡"}};
		JTable table = new JTable(data, title);
		int v = ScrollPaneConstants.VERTICAL_SCROLLBAR_ALWAYS;
		int h = ScrollPaneConstants.HORIZONTAL_SCROLLBAR_ALWAYS;
		JScrollPane js = new JScrollPane(table, v, h);
		add(js);
	}
}

```
![image](https://user-images.githubusercontent.com/54658614/223496792-1dc382f1-5569-4e0d-abb6-f4603d5ff99a.png)




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
