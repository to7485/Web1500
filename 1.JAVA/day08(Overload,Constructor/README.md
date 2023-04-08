## 메서드 오버로드
오버로드은 메서드의 ‘중복정의’ 라고 하며, 하나의 클래스 내에서 같은 이름을 가진 메서드(함수)가 여러개 정의되는 것을 말한다.<br>
메서드명은 대소문자를 비롯해서 반드시 같게 만들어야 하고, 인자들은 인자명을 제외한 인자의 수가 다르든, 인자의 수가 같을 경우 인자의 자료형이 다르든, 다른 메서드에 배치된 인자들과 반드시 다르게 정의되어야 한다.<br>
오버로드은 다양한 메서드들을 같은 이름으로 작업할 수 있다는 의미. 효율성이 높다.<br>

### 클래스 작성
```java
public class Ex1_Overload {
//메서드 오버로드: 메서드의 ‘중복정의’라고 하며 하나의 클래스 내에서 같은 이름을 가진 메서드가
//여러개 정의되는 것

오버로드라고 하는 것 자체가 메서드에만 적용이 되는 것
//**메서드 오버로드의 규칙**
//1) 메서드의 이름이 같아야 함
//2) 파라미터의 개수가 다른 경우
//3) 파라미터의 개수가 같아도 타입이 다른 경우
//4) 파라미터의 개수가 같아도 순서가 다른 경우

오버로드라고 하는 개념을 숙지만 하고 실제로 직접적으로 오버로드 메서드를 만들어서 사용하는 경우가 거의 없기 때문
  
	public void result() {
		System.out.println(“인자가 없는 메서드”);
	//return; 강제로 끝내고 싶을 때는 return을 써도 됨. 대신 void 일때는 return에 아무 값도 		실을 수 없음 사실 void일때는 return을 쓰는 경우도 거의 없고 쓸 필요도 없음
	}

	//메서드 이름이 같기 때문에 오류가 나는게 당연하다.
	pulic void result( int n ) {
		System.out.println(“정수를 인자로 받는 메서드”);
	}
	public void result( char n) {
		System.out.println(“문자를 인자로 받는 메서드”);
	}
	public void result( String s, int n) {
		System.out.println(“문자열, 정수를 인자로 받는 메서드”);
	}
}
질문 아래 코드는 오버로드로 인정 받을까요 아닐까요?? 답 : 인정된다.
public void result( int n , String s) {

} 
	public void result( int n, String s) {
		System.out.println(“정수, 문자열을 인자로 받는 메서드”);
	}


메인클래스 작성
public class Ex2_OverloadingMain {
	public static void main(String[] args) {

	Ex1_Overload ov = new Ex1_Overload();
	ov.result();
	ov.result(10);
	ov.result(‘A’); //인자를 char로 받으면 65를 넣어도 ‘A’가 출력되긴함.
	ov.result(“hi”,10);
	ov.result(10,“hi”);

	여러분들이 지금까지 수업을 듣는동안 알게모르게 오버로드메서드가 있다.

	System.out.printString(“안녕하세요”);
	System.out.printInt(10 + 15);

	오버로드의 장점 메서드를 상황에 따라서 구분할 필요가 없음 예를들어서 그냥 print를 쓰면됨
  

  }
}
```
### 메서드 오버로드 문제
```java
Method클래스를 만들어 각기 다른 기능을 하는 makeBread()메서드를 세 개만드는데,
메인클래스에서 각각의 메서드를 호출했을때의 결과를 보고 로직을 구현해 보자.

//빵을 만들었습니다 <-------------- method 1을 호출해서 나온 결과
//------------------
		
//빵을 만들었습니다 <-------------- method 2을 호출해서 나온 결과
//빵을 만들었습니다.
//요청하신 n개의 빵을 만들었습니다.
//---------------------

//크림빵을 만들었습니다 <-------------- method 3을 호출해서 나온 결과
//크림빵을 만들었습니다.
//요청하신 n개의 빵을 만들었습니다.

Method클래스 생성. 오버로드 메서드 정의
public class Method {
	
	void makeBread()
	{
		System.out.println("빵을 만들었습니다.");
	}

	void makeBread(int count)
	{
		for(int i = 0; i < count; i++)
			System.out.println("빵을 만들었습니다.");
		System.out.println("요청하신 " + count + "개의 빵을 완성했습니다.");
	}

	void makeBread(int count, String name)
	{
		for(int i = 0; i < count; i++)
			System.out.println(name + "을 만들었습니다.");
		System.out.println("요청하신 " + count + "개의 " + name + "을 완성했습니다.");
	}
}

오버로드을 위한 메인클래스 생성
public class MethodMain {
	public static void main(String[] args) {

		Method mh = new Method();
		Scanner sc = new Scanner(System.in);
		
		mh.makeBread(); //예제1
		System.out.println("-------------------------------");
		
		System.out.print(“만들고 싶은 빵의 개수: ”);
		int num = sc.nextInt();
		mh.makeBread(num); //예제2
		System.out.println("-------------------------------");
		
		System.out.print(“만들고 싶은 빵의 개수: ”);
		int num = sc.nextInt();
		System.out.print(“만들고 싶은 빵의 종류: ”);
		String b = sc.next();
		mh.makeBread(num, b);//예제3
	}
}	
```
## 생성자
객체가 생성될 때 메모리 할당 및 멤버변수의 초기화를 목적으로 사용하는 것.<br>
객체가 생성될 때 딱 한번만 호출된다는 것 잊지말긔.<br>
//생성자 개념은 직전에 했던 빵 만들기 클래스와 노트클래스를 새로 만들어서 설명한번 더 해줄 것.<br>

```java
생성자 클래스 작성
public class ConTest {

	//생성자 : 객체가 생성될 때 메모리 할당을 위해 호출되는 영역
	//생성자는 객체가 생성될 때 처음 딱 한번만 자동으로 호출된다.

	//ConTest ct = new ConTest();

	//생성자의 특징
	//클래스와 이름이 완전히 동일하다.
	//반환형이 없다.
	public ConTest(){
		System.out.println("나는 생성자임 ㅋㅋ");
	} 
    	
	public ConTest(String name) {
		System.out.println("name이 " + name + "으로 초기화 됨");
	} 
	//두번째 예제(세번째 할 때 주석처리). 생성자에 파라미터를 넣어서 정의할 수 있음
	
}

메인클래스 정의
public class ConMain {
	public static void main(String[] args) {

	다른 클래스에 있는 메서드 혹은 멤버변수(ssd,cpu,ram) 쓰고싶을 때 클래스를 힙 메모리에
	할당을 받는다.
		
	ConTest ct = new ConTest(); //생성자를 호출한다는 뜻임
	//ct.ConTest(); --> 생성자를 . 찍어서 호출 불가능
	//new라고 하는 키워드는 힙이라고 하는 동네에 집을 지을 땅이 있는지 찾아보는 역할
	//실제로 집을 짓는 역할은 생성자가 함.
}

---------------------------------------------------------------------- 예제 1
Person클래스 생성
public class Person {
	int age;
	String name;
	String tel;

	//예를들어서 나이와 이름, 전화번호를 알아야 친구가 된다고 가정을 해볼게요.
	//이중에 한가지라도 빼먹고 안쓰면 문제가 있는거에요 써먹기 불가능한 깨체가 될수도 있다는거죠.

	//똑같은걸 계속 만드려고 한다면 기본생성자에 값을 넣어놓는것도 좋은 방법
	pbulic Person() {
		age = 40;
		name = “노태문”;
		tel = “010-1111-1111”; 
	}

	//빈공간에서 cntl + space bar 기본생성자 생성
	//임의로 새로운 생성자를 정의한 순간부터 기본생성자는 쓸 수 없다.
	public Person(int age, String, String tel) {
		this.age = age;
		this.name = name;
		this.tel = tel;
	}
	//만든순간 main에서 오류가남 PMain으로 이동
}

PMain클래스 생성
public class PMain{
	public static void main(String[] args) {
		
		Person p1 = new Person(20,“홍길동”,“010”);
		p1.age = 40;
		p1.name = “홍길동”;  --> 인자를 받는생성자 만들고 나서는 주석처리
		p1.tel = “010”;

		Person p2 = new Person();
		p2.name = “홍길순”; //이름만 받고 나이와 전화번호를 받는걸 까먹음

//친구는 다시 물어볼 수라도 있지만 연필,볼펜,마우스,모니터 같은 제품이 만들어질 때 문제가 되요. 길이,모양,단단함,가격 정의를 해야되는데 까먹고 가격을 정의를 안한거야. 이러면 납품되서 팔아야 되는데 못파는 상황이 될 수 있어요. 그냥 폐기물이 되버리는거에요 문제가 되는거죠. 객체를 만들 때 절대로 빼먹으면 안되는 정보들이 있다. 여기서 생성자가 굉장히 좋은 역할을 해줍니다. Person클래스로 이동
}
```
## 생성자 오버로드
```java
생성자 오버로드 클래스 정의
public class Pen{

	int price;
	Stirng brand = “moanmi”;
	String color;
	
	public Pen(int price, String color){
		this.price = price;
		this.color = color;
	}//인자의 자료형이나 인자의 갯수가 달라야 오류가 나지 않는다.
	//일반 메서드 오버로드과 동일
	
	public Pen(){
		price = 1000;
		brand = “monami”;
		color = “white”;
	}
}

생성자 오버로드 메인 클래스 정의
public class PenMain{
	public static void main(String[] args) {
		Pen p1 = new Pen(); //1000원짜리 일반 모나미펜
		Pen p2 = new Pen(10000,“gold”); //만원짜리 금으로된 한정 모나미 펜
	
	}
}
```




## static변수
변수가 static으로 선언되면 객체를 생성하지 않고도 사용할 수 있다.<br>
그리고 현재 static변수를 가지는 객체를 아무리 많이 생성한다고 해도 static변수는 오직 하나만 만들어 진다.<br>
하나만 만들어 진다는 것의 의미는 은행의 이자율을 예로 들어주면 될 듯.<br>

static변수를 사용해서 은행 이자율을 관리해보자.<br>
```java
Bank클래스 정의
public class Bank {
	//static : 객체가 아무리 많이 생성되어도
	//메모리상에 딱 한 개만 만들어지는 변수나 메서드
	private String name = “우리은행”;
	private String point; //은행 위치
	private String tel; //은행 전번
	static float interest = 10f; //은행이자
	
	//생성자 써먹어보자(생성자를 setter처럼 사용)
	public Bank(String point, String tel) {
		this.point = point;
		this.tel = tel;
	}
	
	//결과를 출력할 메서드
	public void printBank(){
		System.out.println("이름 : " + name);
		System.out.println("위치 : " + point);
		System.out.println("전화번호 : " + tel);  
		System.out.println("이자율 : " + interest + “%”);
		System.out.println("-----------------------");
	}
}

BackMain 클래스 정의
public class BankMain {
	public static void main(String[] args) {
		
		Bank bk1 = new Bank("강남", "02-111-2222");
		Bank bk2 = new Bank("분당", "031-333-4444");
		Bank bk3 = new Bank(“대전”, “042-333-3333”);

		//static 변수 클래스 이름으로 접근하는게 보통의 방식이다.
		//객체 생성 없이도 언제든 가져다가 사용할 수 있다.
		Bank.interest = 0.1f;
	
		bk1.printBank();
		bk2.printBank();
		bk3.printBank();	
					
	}
}
```
