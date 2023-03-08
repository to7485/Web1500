ex1_event 패키지 생성

#### Ex1_Event클래스 생성
```java
public class Ex1_Event {

	//이벤트 감지자에서 값을 변경하며 사용할 변수(static으로 선언해둬야 감지자에서 사용이 가능)
	static String radioStr = “”;

	public static void main(String[] args) {

	Frame f = new Frame(“각종 컴포넌트”);

	f.setLayout(null); //자동배치 끄기

	//글자와 관련된 클래스
	Font font = new Font(name,style,size);
	Font font = new Font(“궁서체”,font.BOLD,30);

	//화면에 텍스트를 출력하는 클래스
	Label q1 = new Label(“관심 분야는 무엇입니까?”);
	q1.setFont(font);
	q1.setBounds(10, 30, 400, 50);
	q1.setBackground(Color.YELLOW);

	//다중선택이 가능한 CheckBox생성
	Checkbox movie = new Checkbox(“영화”);
	Checkbox music = new Checkbox(“음악”);

	movie.setBounds(10, 90, 70, 30);
	music.setBounds(80, 90, 70, 30);

	f.add(q1);//프레임에 레이블 추가
	f.add(movie);//프레임에 체크박스가 한 개 추가
	f.add(music);//프레임에 체크박스가 한 개 추가

	//체크박스에 대한 이벤트 감지자
	ItemListener checkListener = new ItemListener() {
		
		@Override
		public void itemStateChanged(ItemEvent e) {
			//체크박스에 쓰여진 텍스트를 가져온다.
			System.out.println(e.getItem().toString());
			String res = “”;
			switch( e.getItem().toString() ) {
		
				case“영화”:
					res = e.getStateChange() == 1 ? “영화선택” : “영화취소”;
					break;

				case“음악”:
					res = e.getStateChange() == 1 ? “음악선택” : “음악취소”;
					break;
			}//switch
			System.out.println(res);
		}
	};



	//체크박스에 감지자 추가
	movie.addItemListener(checkListener);
	music.addItemListener(checkListener);

	//화면에 텍스트를 출력하는 클래스
	Label q2 = new Label(“관심 분야는 무엇입니까?”);
	q2.setFont(font);
	q2.setBounds(10, 130, 400, 50);

	f.add(q2);

	//라디오버튼 만들기
	CheckboxGroup group = new CheckboxGroup();
	Checkbox c1 = new Checkbox(“이과”, group, false);
	Checkbox c2 = new Checkbox(“문과”, group, false);
	
	c1.setBounds(10, 190, 70, 30);
	c2.setBounds(90, 190, 70, 30);

	//프레임에 라디오객체 추가
	f.add(c1);
	f.add(c2);

	//라디오버튼 이벤트 처리를 위한 감지자 생성
	ItemListener radioListener = new ItemListener() {
		
		@Override
		public void itemStateChanged(ItemEvent e) {
			//체크박스에 쓰여진 텍스트를 가져온다.
			radioStr = e.getItem().toString();
			//System.out.println(radioStr);
			JOptionPane.showMessageDialog(f, radioStr + “선택함” );
		}
	};

	//라디오버튼에 이벤트 감지자 등록
	c1.addItemListener(radioListener);
	c2.addItemListener(radioListener);
	f.setBounds(500,100,800,250);
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
![image](https://user-images.githubusercontent.com/54658614/223324889-7d9bd9a4-cb50-4919-af24-a7ac81b984d7.png)


#### ex1_ChoiceTest클래스 생성
```java
public class ChoiceTest {

	static String dayStr = “일요일”;

	public static void main(String[] args) {
		Frame f = new Frame("질문");
	
		//프레임의 화면을 가득 채우는 자동배치를 끈다.
		f.setLayout(null);
		
		//항목선택 상자
		Choice day = new Choice();
		day.add("일요일");
		day.add("월요일");
		day.add("화요일");
		day.add("수요일");
		day.add("목요일");
		day.add("금요일");
		day.add("토요일");
		
		//choice객체의 높이는 안에 추가되어 있는 내용의 크기에 따라 달라지므로
		//높이값인 height 속성은 0으로 둬도 무관
		day.setSize(150, 0);  
		day.setLocation(50, 100); //초이스 위치

맨처음에 일요일로 되어있는데 건드리지 않으면 일요일이 보이고 있어도 선택이 된걸로 간주하지 않아서 아무것도 안건드린 상태라면 변경이 안됐다고 생각을해서 일요일이라고 하는 값이 현재 체크가 되있다는걸 인지를 못해요.

		day.addItemListener(new ChoiceAdapter(f)); <- 지금 메모리에 주소가 있기 때문에 파라미터로 넘긴다. 기본 자료형을 파라미터로 넘기면 주소의 복사본이 넘어가지만 객체가 파라미터로 넘어가면 주소값을 알려준다.
		//만들어진Choice객체인 day를 frame에 추가
		f.add(day);
		f.setBounds(400,100,500,250);
		f.setVisible(true);


메서드 소괄호 안에 이름이 없는 클래스가 만들어지는걸 익명내부클래스라고 하더라. 보통 이벤트처리할 때 많이 사용하고 그리고 인터페이스를 구현하면 추상은 좋든싫든 가지고 있어야 하니까 왜 강제로 오버라이딩이 되는가 그중에서 내가 필요한것들을 정의했을 때 어떤식으로 써먹는가에 대해서 감만 잡고 계시면 되는거에요. 프로젝트 끝나고 나면 잊어먹으실껀데 프로젝트 끝나면 프레임 관련 애들은 사용을 안하기 때문에 이거는 프로젝트 한정 코드라고 보시면 되요. 
		f.addWindowListener(new WindowAdapter() {
			@Override
			public void windowClosing(WindowEvent e) {
				System.exit(0);
			}
		});
	}//main
}
```
![image](https://user-images.githubusercontent.com/54658614/223324935-fc66bed5-bfa5-4744-bc38-44f86377cf63.png)


#### ChoiceAdapter클래스 생성
```java
윈도우 리스너를 제외하면 나머지 감지자들은 어지간하면 추상메서드를 하나만 갖고 있어요.

public class ChoiceAdapter implements ItemListener {

	Frame f; //얘는 new를 한적이 없어요 그렇다는 얘기는 메모리가 할당을 받은적이 없다는 거다. 
  //결론적으로 frame f = null; 연결된 주소가 없다는 의미에 null이 들어가 있다.

	public ChoiceAdapter( Frame f ) {
		this.f = f;
	}

	@Override
	public void itemStateChanged(ItemEvent e) { e 가 초이스에서 요일을 읽어온다.
		ChoiceTest.dayStr = e.getItem().toString();
		JOptionPane.showMessageDialog(f,“오늘: ” + ChoiceTest.radioStr );
    //showMessageDialog는 프레임이 여러개 있을 때 어떤프레임 위에 띄울지 선택을 해야한다. 
    //근데 우리는 프레임을 하나만 갖고 있고, 이럴 때 생성자가 도움이 되요.
	}
}
```
![image](https://user-images.githubusercontent.com/54658614/223325033-f04051f1-07f2-40fd-9b7a-fc932533e61c.png)



ex2_frame_exam 패키지 생성

#### FrameExamMain클래스 생성
```java
public class FrameExamMain {
	public static void main(String[] args) {
		Frame frame = new Frame(“메모장”);
		frame.setBounds(700,200,250,400);
		frame.setBackground(Color.CYAN);
		frame.setLayout(null);//자동배치 끄기

		Font font = new Font(“”,Font.PLAIN,20);

		//키보드에서 넘어온 값을 입력받기 위한 객체 스캐너를 안쓴다고 한 이유
		TextField tf = new TextField();
		tf.setBounds(10,35,180,30);
		tf.setFont(font);

		Button btn_input = new Button(“확인”);
		btn_input.setBounds(190,35,45,30);
		btn_input.setEnabled(false);//버튼 클릭 비활성화

		//키보드에서 여러줄을 동시에 입력받을 수 있도록 하는 클래스
		TextArea ta = new TextArea(미리 입력될 글,0,0,가로,세로 스크롤);
		TextArea ta = new TextArea(“안녕”,0,0,TextArea.SCROLLBARS_VERTICAL_ONLY);
		ta.setBounds(10, 70, 230, 280);
		ta.setEditable(false); //ta에 임의로 값을 추가할 수 없도록 하는 기능
		
		Button btnSave = new Button(“저장”);
		Button btnSave = new Button(“종료”);
		btnSave.setBounds(10,356,110,30);
		btnClose.setBounds(130,356,110,30);

		//tf에 값이 들어가 있는지 확인하여 ‘확인’버튼을 활성화 또는 비활성화
		tf.addTextListener( new TextAdapter(tf, btn_input));
		//확인버튼 클릭시 tf의 값을 ta로 복사해서 넣어주자!
		btn_input.addActionListener(new InputButtonAdapter(tf,ta));

		//종료버튼에 이벤트감지자 등록
		btnClose.addActionListener(new ActionListener() {
			//System.exit(0); 모든 프레임들을 종료
			frame.dispose(); //현재 프레임만 종료
		}
	});

		//저장 버튼을 눌렀을 때  ta의 값을 저장하는 이벤트 감지자 등록
		btn.Save.addActionListener(new ActionListener() {
			@Override
			public void actionPerformed(ActionEvent e) {
				//ta에 쓰여진 내용을 가져오자
				String message = ta.getText();

				//경로를 설정하는 FileDialog
				FileDialog fd = new FileDialog( frame,“ 저장”, FileDialog.SAVE);
				//FileDialog.SAVE 오른쪽 아래 버튼이 저장으로 변경

				fd.setVisible(true);

				//fd를 통해 지정한 저장경로와 파일명을 알아내자
				//(fd.setVisible()보다 아래쪽에 작성할 것!!)
				String path = fd.getDirectory() + fd.getFile();
				System.out.println(path);

				//char기반의 스트림을 생성하여 path경로에 저장
				try{
				FileWriter fw = new FileWriter(path);
				fw.write(message);
				fw.close();
				} catch(Exception e) {
					
				}
			}
		}
	});

		frame.add(tf) //frame에 TextField추가
		frame.add(btn_input);//frame에 Button추가
		frame.add(ta); //frame에 TextArea를 추가
		frame.add(btnSave);
		frame.add(btnClose);

		//생성된 프레임의 사이즈 변경을 막는다.
		frame.setResizable(false);
		frame.setVisible(true);

		f.addWindowListener(new WindowAdapter() {
			@Override
			public void windowClosing(WindowEvent e) {
				System.exit(0);
			}
		});

	}//main
}
```
![image](https://user-images.githubusercontent.com/54658614/223325096-72963286-ad91-487d-949d-c2bb8acfdf5d.png)

---------------------------------------------------------------------------------
##### TextAdapter 클래스 생성
```java
public class TextAdapter implements TextListener {

	private TextField tf;
	private Button btn_input;

	public TextAdapter(TextField tf, Button btn_input) {
		this.tf =tf ;
		this.btn_input = btn_input;
	}

	@Override
	안에 들어가는 값이 지워지든 추가가 되든 변경되면 호출되는 메서드
	public void textValueChanged(TextEvent e) {

		if(tf.getText().equals(“”) ) {
			//tf에 아무것도 쓰여있지 않으면 버튼 비활성
			btn_input.setEnabled(false);	
		} else {
			//tf에 뭔가 쓰여있으면 버튼을 활성
			btn_input.setEnabled(true);
		}
		//System.out.println(“hihi”);
	}
}
```

#### InputButtonAdapter 클래스 생성
```java
public class InputButtonAdapter implements TextListener {

	private TextField tf;
	private TextArea ta;

	public nputButtonAdapter(TextField tf, TextArea ta) {
		this.tf =tf ;
		this.ta = ta;
	}

	@Override
	안에 들어가는 값이 지워지든 추가가 되든 변경되면 호출되는 메서드
	public void actionPerformed(ActionEvent e) {
		//ta내부의 모든 값을 변경하는 메서드 setText	
		//ta.setText(tf.getText());

		//ta가 가진 기존 값에 새로운 값을 이어붙이자(append)
		ta.append( tf.getText() + “\n”);
		
		tf.requestFocus(); //tf로 커서를 이동시킨다.
		tf.setText(“”);

	}
}
```



