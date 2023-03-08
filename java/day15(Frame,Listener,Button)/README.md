화면을 만들고 디자인 하기 위한 awt 패키지의 클래스들을 살펴보자.

### java.awt패키지
java.awt(Abstract Window ToolKit) 패키지는 GUI 프로그램을 개발하기 위한 패키지이다.<br>
사용자 인터페이스를 만들고 그래픽과 이미지를 구현하는데 사용하는 클래스를 포함한다.<br>

GUI(Graphical User Interface)의 약자, 사용자가 편리하게 사용할 수 있도록 입출력 등의 기능을 알기 쉬운 아이콘 형태의 그래픽으로 나타낸것.<br>

ex1_frame 패키지 생성

#### FrameTest 클래스 생성
```java
public class FrameTest {

	public static void main(String[] args) {
		
		Frame frame = new Frame("첫 프레임");//프레임 제목
    frame.setSize(500,300); //프레임의 크기 setSize(너비,높이)
    frame.setLocation(400,400); //프레임이 보여질 위치 setLocation(x좌표,y좌표)
		
		System.out.println(frame.getBounds().getWidth());//double형식으로 반환
		System.out.println(frame.getBounds().height);//int형 속성
		
		frame.setBackground(Color.BLUE);//배경색 지정
		
		frame.setVisible(true);//true가 없으면 화면에 보이지 않음
	}
}
```
![image](https://user-images.githubusercontent.com/54658614/223322809-37031b7c-c3ed-4e60-a693-bd3c58cc29ef.png)


프레임을 상속받는 클래스를 만들고, 이 클래스를 객체화하여 화면에 뿌려보자.

#### MyFrame클래스 생성
```java
public class MyFrame extends Frame{
	public MyFrame() {//생성자
		setSize(500,300);//프레임의 크기 setSize(너비,높이)
    setLocation(400,400);
		setVisible(true);
	}
}
```

#### FrameMain클래스를 만들고 MyFrame을 객체화해 사용해보자.
```java
public class FrameMain {
	public static void main(String[] args) {
		MyFrame fr = new MyFrame();
		fr.setBackground(Color.YELLOW);
		fr.setTitle("네번째 사용자 프레임");	
	}
}
```
![image](https://user-images.githubusercontent.com/54658614/223322864-59041785-573b-4646-ba50-54890613c741.png)

결과는 위와 똑같다.<br>

하지만 우상단에 x버튼을 눌러도 종료가 되지 않는 모습을 볼 수 있다.<br>
버튼은 있지만 껍데기만 있을뿐 종료를 한다는 '기능'이 붙어있지 않기 때문이다<br>

### Listener

ex3_listener 패키지<br> 
앞으로 ActionListener, WindowListener, KeyListener, 등 리스너라고 붙어있는 클래스들을 만나면 100%다 인터페이스라고 생각해도 된다.<br>
리스너라고 하는건 이벤트를 감지하고 싶을 때 x버튼을 누를꺼다 라고 하는건 예측할 수 있어도 실제로 눌렀을 때 어떤 일을 하게 될지는 개발자만 아는거니까<br>
개발자들이 최소화나 최대화, 그리고 X버튼을 눌렀을 때 여기다가 작업만 해주면<br>
그 이후에는 내가 알아서 처리할게 라고 하는 이벤트 처리를 위한 메서드를 제공해주는 인터페이스를 가지고 다 리스너 라고 부른다.<br>

#### WindowListener를 구현하는 TestListener 클래스 생성
```java
public class TestListener implements WindowListener{ 
윈도우 리스너라고 하는 인터페이스를 ‘구현’ 해야한다. 그리고 왜 오류가 나는지 알아야 한다.
인터페이스에 쓸 수 있는거는 상수와 추상메서드밖에 없다.
추상 메서드를 전부다 오버라이딩 해서 자식 클래스의 상황에 맞게 재정의를 해야 한다. 
windowListener인터페이스가 갖고 있는 메서드를 완성시켜줘야할 의무가 있다.
	@Override
	public void windowActivated(WindowEvent e) { }

	@Override
	public void windowClosed(WindowEvent e) { }

	@Override
	public void windowClosing(WindowEvent e) {
		//실행중인 frame의 우상단 x버튼을 클릭하면 호출되는 메서드
		System.out.println("나는 x버튼을 눌렀음");
		System.exit(0); // 열려있는 모든 프레임을 종료하는 메서드
	}

	@Override
	public void windowDeactivated(WindowEvent e) { }

	@Override
	public void windowDeiconified(WindowEvent e) {
		//최소화 상태의 frame이 원래크기로 돌아왔을 때
		System.out.println("최소화 취소");
	}

	@Override
	public void windowIconified(WindowEvent e) {
		//실행중인 frame의 우상단 최소화(_)버튼을 클릭하면 호출되는 메서드
		System.out.println("최소화 되었음");
	}

	@Override
	public void windowOpened(WindowEvent e) { }
	}

	내가 쓸일이 없는 뭔지도 모르는 메서드까지 오버라이딩을 해놔야 된다.
 	영원히 안쓴다는 생각이 들어도 갖고 있어야된다.
 ```

#### FrameMain 클래스 생성
```java
public class FrameMain{
	public static void main(String[] args) {
		Frame f = new Frame(“이벤트 감지자”);

		//f.setSize(300,300); 너비 300에 높이 300 픽셀에
		//f.setLocation(500,300); 위치는 x좌표 500에 y좌표 300픽셀 떨어진곳에 만들거야
		
		//setSize(), setLocation()를 하나로 합쳐놓은 메서드
		f.setBounds(x, y, width, height);

		//frame에 클릭을 감지하는 이벤트 감지자 등록
		//모든 프레임은 addWindowListener라는 메서드를 가지고 있어요.
    
		//WindowListener wl = new WindowListener(); 인터페이스 생성해보면 추상메서드를 메인에서 구현을 하게 된다.
    
		TestListener tl = new TestListener(); 위의 인터페이스 써서 지저분하게 쓸바에야 윈도우리스너 구현하고 있는 클래스를 객체화 하면 좋다.

		//프레임의 우상단 버튼들에 대한 이벤트를 감지하는 감지자
		f.addWindowListener(tl); 인자로 WindowListener 인터페이스만 받음

		f.setVisible(true);
	}
}
```

ex4_listener 패키지

#### MyListener 클래스
```java
public class MyListener implements WindowListener {
	@Override
	public void windowActivated(WindowEvent e) { }

	@Override
	public void windowClosed(WindowEvent e) { }

	@Override
	public void windowClosing(WindowEvent e) {
	}

	@Override
	public void windowDeactivated(WindowEvent e) { }

	@Override
	public void windowDeiconified(WindowEvent e) {
	}

	@Override
	public void windowIconified(WindowEvent e) {
	}

	@Override
	public void windowOpened(WindowEvent e) { }
	}
}
```

#### MyClosing 클래스생성
```java
오버라이딩만 해놓고 x버튼을 클릭했을 때 동작시키기 위한 전용클래스를 만들것이다.
public class MyClosing extends MyListener{

	@Override
	public void windowClosing(WindowEvent e) { //우상단 x버튼을 눌렀을 때 호출되는 메서드만 오버라이딩을 하자.
		super.windowClosing(e); 
		System.exit(0);
	}
}
클래스의 개수가 늘어나긴 했지만 인터페이스 클래스 부분은 전혀 볼 필요가 없어지고 역할이 무었인지 알아보기가 훨씬 편해진다.
```

#### FrameMain 클래스 생성
```java
public class FrameMain{
	public static void main(String[] args) {
		Frame f = new Frame();

		f.setBounds(500,300,400,300); //x좌표,y좌표, width,height

		//MyClosing mc = new MyClosing();

		//익명 클래스 : 이름이 없는 클래스
		//new로 생성된 시점에서 메모리를 잠시 할당받아뒀다가
		//사용이 끝나면 자동으로 제거된다.
		f.addWindowListener(new MyClosing()); //우측 상단에 3개 버튼을 감지하는 감지자

		f.setVisible(true);
	}
}
인터페이스의 추상메서드를 -> 구현을 한 클래스를 -> 상속을 받아서 오버라이딩
x버튼을 누르기 위해서 클래스를 두 개 만들어야 된다.
고슬링 아저씨가 미리 만들어 놓은 어댑터라고 하는 클래스가 있다. 
왜 이렇게 했냐면 상속관계나 인스턴스에 대한 관계를 이해를 하고 계셔야 하기 때문에 보여준것.
```

ex5_window_adapter 패키지 생성

#### Ex1_Frame 클래스
```java
public class Ex1_Frame{
	public static void main(String[] args) {
		Frame f = new Frame(“어댑터 활용하기”);

		f.setBounds(500, 300, 300, 300);

		//f에 우상단 버튼 틀릭 이벤트 감지자 등록
		//메서드의 파라미터 내부에 생성되는 이름없는 클래스
		 -> 익명 내부 클래스 대부분 이벤트 처리할 때 사용된다. 
		f.addWindowListener(new WindowAdapter() {
			@Override
			public void windowClosing(WindowEvent e) {
				System.exit(0);
			};
		});

		f.setVisible(true);
	}
}
```
### Button

ex6_button패키지 생성

#### Ex1_Button 클래스 생성
```java
public class Ex1_Button{
	public static void main(String[] args) {

		Frame f = new Frame(“버튼 생성하기”);

		f.setBounds(500, 300, 300, 300);

		//frame은 기본적으로 add되는 객체를 화면에 가득 채우도록 설계되어 있다.
		//이것을 무시하고 ㅁdd되는 객체들의 위치나 크기값을 지정할 수 있도록 속성을 변경하는 코드가
		//setLayout(null)이다.
		//이것을 ‘프레임의 자동배치를 끈다’라고 표현한다.
		f.setLayout(null);

		//자동배치가 꺼져있는 프레임에 추가될 버튼들의 크기와 위치값을 지정
		btnOK.setBounds(70,90,100,50);
		btnClose.setBounds(230, 90, 100, 50);

		//버튼 생성
		Button btnOK = new Button(“확인”);
		Button btnClose = new Button(“닫기”);

		//생성된 버튼을 frame에 추가
		f.add(btnOK);
		f.add(btnClose);

		f.setVisible(true);

		f.addWindowListener(new WindowAdapter() {
			@Override
			public void windowClosing(WindowEvent e) {
				System.exit(0);
			};
		});

	}//main
}
```
f.setLayout(null)을 하지 않으면 아래의 그림과 같이 버튼이 배치가 되버린다.<br>
![image](https://user-images.githubusercontent.com/54658614/223323313-4d88d0ee-b9bb-403d-9fbe-c60071b6e662.png)


#### Ex2_Button 클래스 생성
```java
public class Ex2_Button{
	public static void main(String[] args) {

		Frame f = new Frame(“버튼 생성하기”);

		f.setBounds(500, 400, 400, 400);
    간혹 한글이 네모로 나오는 경우 프로젝트 우클릭 -> properties -> Run/Debug Settings -> Ex2_Button
    edit -> Arguments 탭으로 이동 -> VM arguments에  -Dfile.encoding=MS949작성 APPLY OK

		//frame은 기본적으로 add되는 객체를 화면에 가득 채우도록 설계되어 있다.
		//이것을 무시하고 add되는 객체들의 위치나 크기값을 지정할 수 있도록 속성을 변경하는 코드가
		//setLayout(null)이다.
		//이것을 ‘프레임의 자동배치를 끈다’라고 표현한다.
		f.setLayout(null);

		//자동배치가 꺼져있는 프레임에 추가될 버튼들의 크기와 위치값을 지정
		btn1.setBounds(70,90,100,50);
		btn2.setBounds(230, 90, 100, 50);

		//버튼 생성
		Button btn1 = new Button(“1”);
		Button btn2 = new Button(“2”);

		//생성된 버튼을 frame에 추가
		f.add(btn1);
		f.add(btn2);

		//버튼들에게 클릭 이벤트 감지자 등록
		btn1.addActionListener(new ActionListener() {
			
			@Override
			public void actionPerformed(ActionEvent e) {
				System.out.println(“1번 버튼 눌렀음”);
			}
		});

		btn2.addActionListener(new ActionListener() {
			
			@Override
			public void actionPerformed(ActionEvent e) {
				System.out.println(“2번 버튼 눌렀음”);
			}
		});



		f.setVisible(true);

		f.addWindowListener(new WindowAdapter() {
			@Override
			public void windowClosing(WindowEvent e) {
				System.exit(0);
			};
		});

	}//main
}
```

![image](https://user-images.githubusercontent.com/54658614/223323158-676376bf-ef9a-4b68-896f-731f7d2edb22.png)

버튼이 많아지게 되면 모든 버튼에 따로따로 감지자를 달아주게 되면 코드가 길어지고 관리가 힘들어지게 된다.

#### Ex3_Button 클래스 생성
```java
public class Ex3_Button{
	public static void main(String[] args) {

		Frame f = new Frame(“버튼 생성하기”);

		f.setBounds(500, 400, 400, 400);

		//frame은 기본적으로 add되는 객체를 화면에 가득 채우도록 설계되어 있다.
		//이것을 무시하고 ㅁdd되는 객체들의 위치나 크기값을 지정할 수 있도록 속성을 변경하는 코드가
		//setLayout(null)이다.
		//이것을 ‘프레임의 자동배치를 끈다’라고 표현한다.
		f.setLayout(null);

		//자동배치가 꺼져있는 프레임에 추가될 버튼들의 크기와 위치값을 지정
		btn1.setBounds(70,90,100,50);
		btn2.setBounds(230, 90, 100, 50);

		//버튼 생성
		Button btn1 = new Button(“1”);
		Button btn2 = new Button(“2”);

		//생성된 버튼을 frame에 추가
		f.add(btn1);
		f.add(btn2);

		//버튼들이 참조할 이벤트 감지자(인터페이스) 생성
		ActionListener al = new ActionListener () {

			@Override
			public void actionPerformed(ActionEvent e) {
			//System.out.println(“안녕”e.getActionCommend());
			//ActionCommend()는 버튼에 쓰여진 내용을 출력하는 메서드
				switch(e.getActionCommend()) {
					case “1”:
						System.out.println(“1번 버튼 클릭됨”);
						break;
					case “2”:
						System.out.println(“2번 버튼 클릭됨”);
						break;
				}//SWITCH
			}
		};

		btn1.addActionListener(al);
		btn2.addActionListener(al);		

		f.setVisible(true);

		f.addWindowListener(new WindowAdapter() {
			@Override
			public void windowClosing(WindowEvent e) {
				System.exit(0);
			};
		});

	}//main
}
```
각 버튼을 누를 때 콘솔에 출력이 된다
![image](https://user-images.githubusercontent.com/54658614/223324771-63245873-10ec-47fa-92ce-df40ff6d7739.png)

