### 추상 메서드와 추상 클래스

추상메서드의 구성 : [접근제한] abstract [반환형] [메서드명](); //{ }없이 ;으로 마무리됨<br>
		               abstract [접근제한] [반환형] [메서드명](); //{ }없이 ;으로 마무리됨<br>
완성된 단계가 아닌 미완성적 개념으로 두고, 나중에 타 클래스 내에서 재정의하여 사용할 수 있도록 만드는 것.<br>

추상메서드가 한 개 이상 정의되어 있는 클래스를 추상 클래스라고 하는데,<br>
추상 클래스 또한 abstract를 통해 자신이 추상클래스임을 명시해줘야 한다.<br>
추상클래스의 구성 : [접근제한] abstract class [클래스명]{ }<br>

#### AbsParent정의(abstract)
```java
abstract public class AbsParent {
	//추상메서드를 한갤라도 가지고 있는 클래스는
	//abstract키워드를 붙여 추상클래스로 정의해둬야 한다.

	int value = 100;

	//get space+cntl 누르면 완성 가능
	public int getValue(){ // 일반적인 메서드
		return value;
	}
	그동안 우리가 만들던 일반적인 메서드들과 가장 큰차이점이 뭘까요??
	// 추상 메서드는 body가 없기때문에 이를 "미완성적 개념"이라 한다.

	//추상 메서드는 body( { } )가 없다. - abstract로 시작한다.
	abstract public void setValue(int n);
	//public abstract void setValue(int n);
	
	// 그러므로 이 미완성적 개념을 자식이 물려
	// 받아서 완성 시켜야하는 것이 하나의 조건이 된다.
}
```

#### AbsChild클래스 정의
```java
public class AbsChild extends AbsParent{
	
	//추상 클래스를 상속받은 자식 클래스는
	//부모가 가지고 있는 추상 메서드(미완성)를 무조건 받아두어야 한다.
	//재정의 할 필요는 없지만 오버라이딩 해서 가지고는 있어야 한다는 의미.

	추상클래스에서 만든 메서드를 몸체까지 강제로 오버라이딩이 됨.
	무조건 받아야 하고 자식클래스의 상황에 맞게 내용을 정의할 수 있다.
	재정의할 필요 없다면 그냥 받아만 둬도 상관이 없다.
	@Override
	public void setValue(int n) {
		System.out.println(“추상메서드 재정의함”);
	}
```	
	
#### AbsMain정의
```java
public class AbsMain {
	public static void main(String[] args) {
		//추상클래스는 인스턴스를 직접 가질 수 없다.
		//즉, 객체화 시킬 수 없다는 것.
		AbsParent ap = new AbsParent(); //오류확인 후 주석
			
		// 그러므로 추상클래스는 자신의 기능을 자식이 완성 시키도록 조건부 
		// 상속하여 자식클래스가 생성될 때 객체화된다.
		AbsChild a1 = new AbsChild();
		a1.setValue(20);
		System.out.println(a1.getValue());
	}
}
```
#### AbsClass정의
```java
public abstract class AbsClass {
	int value = 100;

	public int getValue(){
		return value;
	}

	public abstract int changeValue(); // 추상메서드
}
```
#### AbsChild1정의
```java
public class AbsChild1 extends AbsClass{

	@Override
	public int changeValue() {
		return value += 10; 
		//value는 부모의 멤버. 상속 받았기 때문에 사용 가능
	}
}
```
#### AbsChild2정의
```java
public class AbsChild2 extends AbsClass{
	@Override
	public int changeValue() {
		return value -= 3;
	}	
}
```
#### AbsMain정의
```java
public class AbsMain {
	
	public static void main(String[] args) {
		// 추상클래스는 객체화 할 수 없다.
		AbsClass a1 = new AbsClass(); //확인 후 주석

		//추상 클래스는 자식클래스에 의해 객체화 된다.
		AbsChild1 ac1 = new AbsChild1();
		System.out.println(ac1.changeValue());
		↑여기까지 일단 결과로 110이 나오는 이유먼저 설명

	}
}
```
<hr>

우리가 라면을 끓입니다.<br>
공통적으로는 면과 스프를 넣죠? 설마 물 안넣진 않았겠죠.<br>
그 다음에는 치즈를 넣을지 떡을 넣을지 만두를 넣을지 알수가 없기 때문에 각자에 취향에 맞춰서 해야겠죠??<br>
그래서 공통적인 부분인 스프랑 면만 딱 넣어주는거에요 그리고 알아서 먹고싶은대로 넣는게 추상화입니다.<br>

#### Ramen 클래스 생성
```java
abstract public class Ramen {
	String nudle = "면";
	String soup = "스프";
	
	abstract public void makeRamen();
}
```
#### CheeseRamen 클래스 생성
```java
public class CheeseRamen extends Ramen{

	String cheese = "치즈";
	
	@Override
	public void makeRamen() {
		System.out.printf("%s %s %s\n",nudle,soup,cheese);
		
	}
}
```
#### ManduRamen 클래스 생성
```java
public class ManduRamen extends Ramen{
	
	String mandu = "만두";
	
	@Override
	public void makeRamen() {
		System.out.printf("%s %s %s\n",nudle,soup,mandu);
		
	}
}
```

#### RamenMain 클래스 생성
```java

public class RamenMain {
	public static void main(String[] args) {
		
		CheeseRamen cr = new CheeseRamen();
		ManduRamen mr = new ManduRamen();
		
		cr.makeRamen();
		mr.makeRamen();
		
	}//main
}
```

마지막 예제! 책에 아주 잘 나와있는게 있어서 그걸 한번 해볼게요.<br>
스타크래프트 다들 아시는지 모르겠네요. 전 잘 모릅니다.<br>
근데 보면 세 개의 종족에 지상, 공중유닛이 분리되어 있고.... 유닛마다 공격력이나 방어력이 모두 다르잖아요???<br>

모든 유닛은 상대방에게 ‘공격을 당한다’라는 액션을 가지고 있지만, 공격당할 때 감소하는 에너지의 수치는 차이가 있을 겁니다.<br>

이렇게 공통적인 기능이 있으나, 내부에서 구현되어야 하는 코드가 다를 때 사용할 수 있는 아주 괜찮은 예제인 것 같습니다.<br>

#### Unit클래스 정의
```java
public abstract class Unit {
	String name;//이름
	int energy;//체력
	
	//유닛이 공격을 당했을 때 체력 감소량을 관리하기 위한 메서드
	//유닛마다 체력 감소량이 다르기 때문에 추상메서드로 정의했다.
	abstract public void decEnergy();
	
	public int getEnergy() {
		return energy;
	}
}
```
#### Terran클래스 정의
```java
public class Terran extends Unit{

	//공중유닛 이면 true, 지상유닛이면 false
	boolean fly;
	
	public Terran(String name, int energy, boolean fly) {
		super.name = name;
		super.energy = energy;
		this.fly = fly;
	}//생성자 오버로딩.
	
	
	@Override
	public void decEnergy() {
		energy -= 3;
	}	
}
```

#### Protoss클래스 정의
```java
public class Protoss extends Unit{

	boolean fly;
	
	public Protoss(String name, int energy, boolean fly) {
		super.name = name;
		super.energy = energy;
		this.fly = fly;
	}
	
	@Override
	public void decEnergy() {
		energy--;
	}
}
```

#### Zerg클래스 정의
```java
public class Zerg extends Unit{

	boolean fly;
	
	public Zerg(String name, int energy, boolean fly) {
		super.name = name;
		super.energy = energy;
		this.fly = fly;
	}//생성자 오버로딩.
	
	@Override
	public void decEnergy() {
		energy -= 10;
	}
}
```

#### UnitMain클래스 정의
```java
public class UnitMain {
	public static void main(String[] args) {
		Terran t1 = new Terran("해병", 100, false);
		t1.decEnergy();//재정의한 decEnergy()호출
		System.out.println("t1의 energy : " + t1.getEnergy());
		
		Zerg z1 = new Zerg("무리군주", 200, true);
		z1.decEnergy();
		System.out.println("z1의 energy : " + z1.getEnergy());
		
		Protoss p1 = new Protoss("거신", 250, false);
		p1.decEnergy();
		System.out.println("p1의 energy : " + p1.getEnergy());
	}
}
```
### 인터페이스
인터페이스는 앞에서 배운 추상클래스와 매우 유사하지만, 서비스 요청에 따른 중개자 역할을 하는 것과 같다.<br>

중국집에 갔다고 치자. 우리가 직접 주방에 가서 메뉴를 주문하지는 않잖아요??<br>
메뉴판을 통해서 메뉴를 정하고 주문을 해야되요<br>

메뉴를 보고 골라서 직원에게 주문을 하면 주방에서 요리를 만들어서 고객에게 제공하는 것처럼 인터페이스는 고객이 호출할 수 있는 서비스의 목록이라고 할 수 있다.<br>

메뉴판에는 분명 짜장면이 있는데 시켜놓고 보니 주방에 밀가루가 없네요??<br>
알고 보니까 짜장면을 만들지 못하는 식당이었다. 라는 상황이 발생하면 안되기 때문에,<br>
식당(인터페이스를 구현할 클래스)에는 메뉴판(인터페이스)에 있는 서비스가 하나도 빠짐없이 구비되어 있어야 한다.<br>
예제로 확인하시죠. 추상처럼 메서드를 오버라이딩 하는건 맞는데 인터페이스는 조금 더 강제성이 있어요.<br>

```
인터페이스의 구성
[접근제한] interface 인터페이스명{
   상수;
   추상메서드;
}
```
프로젝트에서 마우스 우측클릭, new -> interface를 통해 인터페이스 생성<br>

#### InterTest 인터페이스 정의
```java
public interface InterTest {
	//인터페이스에는 상수와 추상메서드 이외에는 아무것도 들어갈 수 없다
	final int A = 100; //final로 만든 변수는 대문자로 만든다.
	abstract int getA();
}
```
#### InterChild 클래스 정의
```java
public class InterChild implements InterTest{
	//인터페이스를 구현하려면
	//구현하려는 클래스에서 implements예약어를 사용한다.
	
	@Override
	public int getA() {
		return A; //InterTest의 상수 A를 반환
	}
}
```
#### InterMain 클래스 정의
```java
public class InterMain {
	
	public static void main(String[] args) {
		
		InterChild ic = new InterChild();
		System.out.println("getA() : " + ic.getA());
	}
}
```
### 인터페이스간의 상속

#### Kitchen 클래스 생성
```java
public class Kitchen implements //Menu1, //Menu2, Menu3 {
	@Override
	public String jajang() {
		// TODO Auto-generated method stub
		return "중면 + 춘장 + 완두콘";
	}

	@Override
	public String jjambbong() {
		// TODO Auto-generated method stub
		return "중면 + 홍합 + 야채;
	}

	@Override
	public String tangsuyuck() {
		// TODO Auto-generated method stub
		return "돼지고기 + 당근 + 갖은양념";
	}

	@Override
	public String boggembab() {
		// TODO Auto-generated method stub
		return "춘장 + 달걀 + 쌀";
	}

}
```
#### Menu1 인터페이스 정의
```java
public interface Menu1 {
	abstract String jajang();

	//abstract는 생략되어도 interface안에서는 자동으로 추상으로 인식.
	String jjambbong();
}
```

#### Menu2 인터페이스 정의
```java
public interface Menu2 {
	abstract String tangsuyuck();//탕수육 철자 모르겠다;;ㅋ
}
```
#### Menu3 인터페이스 정의
```java
public interface Menu3 extends Menu1, Menu2{
	//인터페이스는 일반클래스 상속이 불가능하고 인터페이스만 상속이 가능하다.
	//인터페이스는 구현능력이 없기 때문에 다중상속이 가능하다
	abstract String boggembab();//보끔밥....
}
```
#### InterMain 클래스 정의
```java
public class Kitchen implements Inter_Menu3{
	public static void main(String[] args) {

		Kitchen k = new Kitchen();

		System.out.println("--우리집 메뉴판--");

		Inter_Menu1 im1 = im;
		Inter_Menu2 im2 = im;
		Inter_Menu3 im3 = im;
		//InterMain의 객체인 im을 Inter_Menu에 대입
		//오류가 날 것 같지만 그렇지가 않다.
		//im이 구현하고 있는 Inter_Menu3이라는 인터페이스가
		//Inter_Menu1과 Inter_Menu2에게서 상속받은 것이므로.

		//하지만 사용 범위가 변환된 각 인터페이스 내에 정의된 메서드들로 
		//국한됨을 기억하자.

		System.out.println(im.jajang());

		//im1객체에서는 jjambbong()과 jajang()만 호출할 수 있다.
		System.out.println(im1.jjambbong());

		//im2객체에서는 tangsuyuck()만 호출할 수 있다.
		System.out.println(im2.tangsuyuck());

		//im3객체에서는  boggembab(), jjambbong(), 
		//	jajang(), tangsuyuck()전부 호출 가능
		System.out.println(im3.boggembab());
	}
}
```
### Try – Catch(예외처리)
자바에서 프로그램이 실행하는 도중에 예외(에러)가 발생하면 그 시점에서 프로그램이 강제적으로 종료된다.<br>
이것은 올바른 판단이지만, 때로는 예상할 수 있는 가벼운 오류가 있거나, 예외가 발생했을때도 프로그램을 종료하지 않고 이후의 작업을 진행해야 할 때가 있다.<br>
예외처리를 통해 프로그램의 비정상적인 종료를 줄이고 정상적으로 프로그램이 계속 진행될 수 있도록 할 수 있다.<br>

#### Ex1_TryCatch 메인클래스 정의
```
public class Ex1_TryCatch {
	public static void main(String[] args) {
//자바에서 프로그램이 실행되는 도중 예외(오류,에러,버그)가 발생하면 그 시점에서
// 프로그램이 강제로 종료된다. 이런 경험 많죠? 이게 올바른 판단인게 종료가 안되면 더 큰문제가 발생할 수 있다.
//이것은 올바른 판단이지만, 때로는 예상가능한 오류가 있거나 오류 발생시 이를 무시하고 이후의 작업을 진행해야 하는 경우가 있다.
//예외처리를 통해 프로그램의 비정상적인 종료를 줄여보자.

예상은 하지만 막을 수 없는 오류란? 키보드에서 값 받을 때 원하는 타입이 아닌 다른 타입으로 받으면 나는 오류 근데 우리가 막을 수 없음

정수를 0으로 나누려고 시도를 하면 거의 모든 프로그램 언어에서는 오류가 난다.
계산기 코드를 짜는데 0으로 나누면 오류가 남 나누기를 하면 0을 쓰면 안돼!
혹은 안내를 해주던지...
		
		int result = 0; 
//try 구역과 catch 구역은 지역변수 개념으로 작용해서 try바깥에 만들 것.
		char[] arr = new char[3];

Exception이라는 말이 들어가면 예외
ArithmeticException -> 뭔가 정수나 실수값을 0으로 나눴을 때 출력되는 예외 코드

코드는 위에서 아래로 진행하기 때문에 오류가 나면 오류가 난 부분부터 위로 훑어보자
오류가 난 것 같은 코드 위아래로 try catch문을 만든다.

대부분의 오류를 잡아주긴 하지만 오류잡기 귀찮다고 모든 코드를 try안에 작성하면 사기꾼이 되는거죠.
오류가 나지 않는 정상적인 코드를 포함해서 try에 작성을 하게되면 오류가 나는 코드를 판단하지 못하고 튕겨져나가버리는 경우가 있어서 오류가 나는 코드를 중점으로 try에 작성할 것.
	try {
		result = 10 / 0; //정수를 0으로 나누려고 하는 오류.
		arr[3] = ‘A’ //없는 index에 값을 넣으려고 하는 오류
	} catch (Exception e) { 
//Exception 이라는 클래스는 자바에서 발생할 수 있는 모든 예외에 대한 정보를 갖고 있는 클래스
		e.printStackTrace();//어떤 오류가 발생하는지 알려줌
		try안에서 어느 부분에서 오류가 났는지 알려줌 

//try영역에서 오류가 발생하면 catch영역으로 이동하여 코드를 수행한 뒤 빠져나간다.
		//System.out.println("에러발생");
		System.out.println("0으로 나눌 수 없습니다.");
	}
		
	System.out.println(result);
	
    }
} 
```
#### Ex2_TryCatch 클래스 작성
```java
public class Ex2_TryCatch {
	public static void main(String[] args) {
		
		int res = 0;
		int[]arr = new int[2];
		try {
			res = 10/0;
			arr[2] = 10;
		} catch (ArithmeticException e) {
			System.out.println("0으로 나눌 수 없습니다.");

		} catch ( ArrayIndexOutOfBoundsException e) {
			System.out.println(“존재하지 않는 index로의 접근입니다.”);
		}finally {
			//try영역에서의 예외 발생 여부와 관계 없이
			//마지막에 반드시 호출되는 영역
예를들면 c드라이브에 파일을 갖고 오고 싶어요 a.txt를 가져오고 싶은데 파일이 없으면 오류가 나요.
			System.out.println(“finally”);
		}
		System.out.println(res);
	}
}
```
#### TryCatchEx2 클래스 작성
```java
public class TryCatchEx2 {
	public static void main(String[] args) {
		
		int res = 0;
		int[] var = {10, 20, 30};
		try {
			for(int i = 0; i <= var.length; i++)
				System.out.println("var[" + i + "] = " + var[i]);
		} catch (Exception e) {
			
			System.out.println("오류발생");
		}
		System.out.println("프로그램 끝");
	}
}
```



```java
문제:
키보드에서 정수를 입력받도록 하고, 정수 이외의 값이 입력되었다면
‘정수만 입력가능’이라는 메시지를 출력하자.

결과:

//정수를 입력 받은 경우
정수 : 100
입력받은 수 : 100


//정수를 입력 받지 않은 경우
정수 : aaa
정수만 입력가능


풀이 :
public class Try_Main {
	public static void main(String[] args) {
		
		System.out.print("정수 : ");
		Scanner sc = new Scanner(System.in);
		
		try {
		int n = sc.nextInt();
		//오류가 발생하면 출력문을 실행하지 않고 catch로 넘어간다.
		System.out.println(“입력받은 수: ” + n);
		} catch (Exception e) {
			System.out.println(“정수만 입력 가능함");
		} 
			
	}//main
}
```

```java
문제:
//정수 : 100
//결과 : 100

//정수 : aab
//결과 : aab은(는) 정수가 아닙니다.

public class Ex4_TryCatch {
	public static void main(String[] args) {
		
		System.out.print("정수입력 : ");
		Scanner sc= new Scanner(System.in);
		String str = “”;
		
		try {
			int n = sc.nextInt();
			str = sc.next();
			int num = Integer.parseInt(str);
			System.out.println(“결과 : ” + num);
		} catch (Exception e) {
			String name = sc.nextLine();
			System.out.println(str + "은(는) 정수가 아닙니다.");
		}		
	}
}
```

### 열거형(enum)
열거형은 상수를 가지고 생성되는 객체들을 한곳에 모아둔 하나의 묶음이다.<br>
변수를 사용자가 지정한 이름으로 0부터 순차적으로 증가시켜준다.<br>


#### EnumMain클래스 정의
```java
public class EnumMain{

	public enum Item{
		Start, Pause, Exit
	}

	public static void main(String[] args) {

		while(true){
			System.out.println("0 : 게임시작");
			System.out.println("1 : 일시정지");
			System.out.println("2 : 게임종료");

			Scanner scan = new Scanner(System.in);
			int n = scan.nextInt();

			Item start = Item.Start;
			Item exit = Item.Exit;
			Item pause = Item.Pause;

			if(n == start.ordinal())
				System.out.println("게임이 시작됨");

			else if(n == pause.ordinal())
				System.out.println("게임이 일시정지됨");

			else{
				System.out.println("게임종료");
				return;
			}
		}	
	}
}
```
--------------------------------------------------------------------예제
### 스레드
스레드는 독립적인 실행단위입니다.<br>
하나의 프로세스 안에서 두 가지 이상의 일을 하도록 하는것<br>
우리가 한글 문서를 작성 하면서 프린트를 사용해서 인쇄하는걸 동시에 할 수 있는 것.<br>
혹은 인터넷을 하면서 음악을 듣는 것처럼 한번에 두가지 이상의 프로세스(운영체제에서 실행중인 하나의 프로그램)를 실행 가능하게 해 준다.<br>
실제로 동시에 두 개가 실행되는 것은 아니고, 운영체제 내부에서 동시에 돌아가는 것처럼 보이도록 아주 빠르게 번갈아서 스레드를 실행하는 것.<br>

프로세스(Process) : 실행중인 프로그램<br>
스레드(Thread) : 프로세스에서 작업을 수행하는 것<br>

#### 프로세스가 실행되는 방식
1. 시간 분할방식 : 모든 프로세스에게 동일한 시간을 할당하고 골고루 실행되는 방식<br>

2. 선점형 방식: 각각의 프로세스에게 우선 순위를 부여하고 우선순위가 높은 순으로 실행되는 방식<br>

스레드의 생성<br>

jvm이 스레드 처리시 하는 일 -> 스레드 스케쥴링<br>
- 스레드가 몇 개 존재하는지
- 스레드가 실행되는 프로그램 코드의 메모리 위치가 어디인지
- 현재 스레드의 상태는 무엇인지
- 스레드의 우선순위는 몇인지

이런건 jvm이 해주기 때문에 우리가 따로 신경쓸 필요가 없어요.

개발자가 스레드 처리시 하는 일
- 자바 스레드로 작동할 작업이 무엇인지 코드로 작성
- 스레드 코드가 실행할 수 있도록 JVM한테 요청

<hr>

#### 스레드의 생성

스레드 작업 코드 작성 방법에는 두가지가 있다.

1) Thread 클래스 상속<br>

#### ThreadTest 클래스 생성

```java
public calss ThreadTest extends Thread{
	public void run() { //run()메서드 오버라이딩
		//작업할 내용
	}
}
```

2) Runnable 인터페이스 구현<br>

#### RunnableTest 클래스 생성
```java
public calss RunnableTest implements Runnable{
	public void run() { //run()메서드 오버라이딩 필수!
		//작업할 내용
	}
}
```
#### ThreadMain 클래스 생성
```java
스레드 클래스 상속시 스레드 코드가 실행할 수 있도록 JVM에 요청
public class ThreadMain{
    public static void main(String[]args){
	ThreadTest t1 = new ThreadTest(); //객체생성
	t1.start();

	러너블 인터페이스 구현시 스레드 코드가 실행할 수 있도록 JVM에 요청
	ThreadTest t2 = new ThreadTest(); //객체 생성
	Thread t = new Thread(t2);
	t.start();
    }
}
```
#### ex2_Thread 패키지 만들기

#### MyThread 클래스 정의
```java
public class MyThread extends Thread{

	@Override
	public void run() {
		//프로세스의 독립적인 수행을 위한 영역
		for (int i = 0; i < 10; i++) {
			System.out.println("스레드 진행 중"+i);
		}
	}
}
```
#### MyThread2 클래스 정의
```java
public class MyThread2 implements Runnable{
	@Override
	public void run() {
		//프로세스의 독립적인 수행을 위한 영역
		for (int i = 0; i < 10; i++) {
			System.out.println("러너블 진행중"+i);
		}
	}
}
```

#### ThreadMain클래스 정의
```java
public class ThreadMain {
	public static void main(String[] args) {
		
		MyThread t = new MyThread();
		t.start();//run()메서드를 호출하는 메서드
		//t.run();은 run()메서드를 독립적으로 수행하지 못한다.
		//즉, 일반 메서드처럼 호출해버림.
		//run()메서드의 내용을 별개로 수행하고 싶다면
		//t.start()를 이용해야 함.

		MyThread2 th2 = new MyThread();
		Thread t = new Thread(th2);
		t.start();
		
		for(int I = 0; i< 10; I++) {
			System.out.println(“메인함수 실행중” + I);
		}
	}
}
```
#### ex3_Thread 패키지 생성

#### MainThread 클래스 생성 – 메인 메서드도 스레드 라는걸 증명
스레드 클래스에서 제공하는 현재 스레드의 이름과 상태 우선순위를 확인해보려고 한다.<br>

```java
public static void main(String [] args) {

	ThreadName tn = new ThreadName();
	tn.start();

	System.out.println(“현재실행되고있는스레드의이름: Thread.currentThread().getName());
	System.out.println(“현재실행되고있는스레드의상태: Thread.currentThread().getState());
	System.out.println(“현재실행되고있는스레드의우선순위: Thread.currentThread().getPriority);
}
```

#### ThreadName 클래스 생성

```java
class ThreadName extends Thread {
	@Override	
	public void run() {
		this.setName(“Thread3”);
		System.out.println(“현재실행되고있는스레드의이름: Thread.currentThread().getName());
	System.out.println(“현재실행되고있는스레드의상태: Thread.currentThread().getState());
	System.out.println(“현재실행되고있는스레드의우선순위: Thread.currentThread().getPriority);
	}
}
```

### 스레드의 라이프 사이클
스레드는 현재 상태에 따라 네 가지 상태로 분류할 수 있으며, 상태가 변화하는 주기를 Life Cycle이라고 한다.<br>

#### 스레드 상태
new : 스레드가 new를 통해서 객체화 된 상태, Runnable이 될 수 있는 상태이며 아직 대기열에 올라가지 못한 상태<br>

Runnable : start()메서드가 호출되면 new 상태에서 Runnable상태가 된다. Runnable상태가 되면 실행할 수 있는 상태로 대기하게 되며 스케줄러에 의해 선택되면 run()메서드를 바로 수행한다.<br>

Blocked : 실행 중인 스레드가 sleep(),join()메서드를 호출하게 되면, Blocked상태가 된다.<br>
Blocked상태가 되면 스케줄러에 의해 선택받을 수 없다.<br>

Dead : run()메서드의 실행을 모두 완료하게 되면 스레드는 Dead상태가 된다. 할당받은 메모리와 정보 모두 삭제된다.<br>

----------------------------------------------------------------------------
#### sleep() 스레드를 지정한 시간동안 block상태로 만든다 시간은 1000분의 1초 까지 결정할 수 있으며, 지정된 시간이 지나면 다시 runnable(실행가능한)상태로 돌아간다.

#### ex4_Thread 패키지 생성

#### SleepThread클래스 생성
```java
public class SleepThread extends Thread{
	public void run() {
		System.out.println(“카운트다운 5초”);
		for(int I = 5; i>=0; I--) {
			if(i!=0) {
				try{
					Thread.sleep(1000);//1000당 1초
				} catch(Exception e) {

				}//catch
			}//if
		}//for
		System.out.println(“종료!”);
	}
}
```
#### SleepMain 클래스 생성
```java
public class SleepMain{
	public static void main(String[] args) {
		SleepThread st = new SleepThread();
		st.start();
	}
}
```

#### yield() : 자신의 시간을 양보하는 메서드, 스레드가 작업을 수행하던 중yield()를 만나면, 자신에게 할당된 시간을 다음 차례 스레드에게 양도

#### YieldTest1 클래스 생성
```java
public class YieldTest1 implements Runnable{
	public void run() {
		for(int I = 0; i<30; I++) {
			System.out.println(“★”);
			Thread.yield();
		}
	}
}
```
#### YieldTest2 클래스 생성
```java
public class YieldTest2 implements Runnable{
	public void run() {
		for(int I = 0; i<30; I++) {
			System.out.println(“☆”);
		}
	}
}
```

#### YieldMain 클래스 생성
```java
public class YieldMain{
	public static void main(String[] args) {
		YieldTest1 yt1 = new YieldTest1();
		YieldTest2 yt2 = new YieldTest2();
		Thread t1 = new Thread(yt1);
		Thread t2 = new Thread(yt2);
		t1.start();
		t2.start();
	}
}
```

#### join() : 특정한 메서드가 작업을 먼저 수행할 때 사용, 실행 순서를 지켜야 하는 스레드들을 제어한다.

#### JoinTest1 클래스 생성
```java
public class JoinTest1 implements Runnable{
	public void run() {
		for(int I = 0; i<10; I++) {
			System.out.println(“t1:” + i);
			
		}
		System.out.println(“<<t1완료>>”);
	}
}

#### JoinTest2 클래스 생성
```java
public class JoinTest2 implements Runnable{
	public void run() {
		for(int I = 0; i<10; I++) {
			System.out.println(“t2:” + i);
		}
		System.out.println(“<<t2완료>>”);
	}
}
```
#### JoinMain 클래스 생성
```java
public class JoinMain{
	public static void main(String[] args) {
		JoinTest1 jt1 = new JoinTest1();
		JoinTest2 jt2 = new JoinTest2();
		Thread t1 = new Thread(jt1);
		Thread t2 = new Thread(jt2);
		t1.start();

		try{
		    t1.join();//t1을 제외한 나머지 스레드 block
		} catch ( Exception e) {

		}
		t2.start();
		try{
		    t2.join();//t2을 제외한 나머지 스레드 block
		} catch ( Exception e) {

		}
		for(int I = 0; i<10; I++) {
			System.out.println(“메인스레드:” +i);
		}

	}
}
```

### 데몬쓰레드
데몬 쓰레드는 다른 일반 쓰레드의 작업을 돕는 보조적인 역할을 수행하는 쓰레드이다.
함께 구동중인 일반 스레드가 종료되면 데몬스레드도 함께 종료된다.
예를들어 문서를 작성하는 도중에 3초 간격으로 자동 세이브가 필요하다고 가정하여 코드를 작성해 보자.

#### DaemonMain 클래스 생성
```java
public class DaemonMain implements Runnable{
	
	static boolean autoSave = false;
	
	public static void main(String[] args) {
		
		DaemonMain dm = new DaemonMain();
		Thread t = new Thread(dm);
		
		
		
		t.setDaemon(true);//t라는 스레드가 데몬스레드임을 명시하는 메서드.
				  //메인스레드가 종료될 때 함께 종료되도록 한다.
		
		
		t.start();//run()메서드 시작
		
		for(int i = 1; i <= 15; i++){
			
			try {
				Thread.sleep(1000);
				
			} catch (Exception e) {
				// TODO: handle exception
			}
			
			System.out.println(i);
			
			if(i == 3)//3초 뒤에 자동세이브 시작
				autoSave = true;
		}
		
		System.out.println("프로그램 종료");
	}

	@Override
	public void run() {
		
		while(true){
			
			try {
				Thread.sleep(3000);
				
			} catch (Exception e) {
				// TODO: handle exception
			}
			
			if(autoSave == true)
				System.out.println("자동저장을 수행합니다.");			
		}
	}//run()
}
```

```java
문제 : 
스캐너를 이용하여 키보드에서 숫자를 입력받고
스레드에서 입력받은 숫자가 1씩 감소하다가 0이 되었을 때
“종료”라는 메시지와 함께 스레드를 빠져나오도록 만들어보자.

public class ThreadCount implements Runnable{

	private int n;
	
	public ThreadCount(int n) {
		this.n = n;
	}

		public void run() {		
		for(int i = n; i >= 0; i--){
			
			try {
				System.out.println(i);
				Thread.sleep(1000);
				
			} catch (Exception e) {
			}
		}
		System.out.println("종료");
	}
```

#### Main클래스 생성
```java
	public static void main(String[] args) {

		System.out.print("값을 입력 : ");
		Scanner scan = new Scanner(System.in);

		ThreadCount t = new ThreadCount(scan.nextInt());
		Thread tt = new Thread(t);		
		tt.start();
	}

}
```


------------------------------------------------------------------예제(문제)6

(심심풀이) Thread.sleep()을 이용하여 <br>
희한한 달리기 게임을 만들어보자ㅎㅎ(함께 해보기)<br>
```java
public class Runners {
	public static void main(String[] args) {
		
		int[] playerRandom = new int[4];
		String[] playerJump = {"", "", "", ""};
		
		boolean finish = false;
		int finishPlayer = 0;
		
		while (true) {
			
			//도약거리
			for(int i = 0; i < playerRandom.length; i++){
				playerRandom[i] = new Random().nextInt(3);
			}
			
			//0.1초 간격으로 휴식
			try {
				
				Thread.sleep(100);
				
			} catch (Exception e) {
				// TODO: handle exception
			}
			
			//각 선수에게 도약거리 적용
			for(int i = 0; i < playerJump.length; i++){
				
				switch (playerRandom[i]) {
				case 0:
					playerJump[i] += "";
					break;

				case 1:
					playerJump[i] += " ";
					break;
					
				case 2:
					playerJump[i] += "  ";
					break;
				}	
			}//for
			
			//달리기
System.out.println("--------------------------------------------");
			for(int i = 0; i < playerJump.length; i++){
				
				System.out.print(playerJump[i]);
				System.out.println(i + 1 + "옷");
				
				if(playerJump[i].length() >= 50){
					finishPlayer = i + 1;
					finish = true;
				}
			}//for	System.out.println("--------------------------------------------");	
			//결산
			if(finish){
				System.out.println("winner - " 
							+ finishPlayer);
				break;
			}
		}//while
	}//main end
}//class end
```
```
자바문제 –스레드(암산퀴즈) 출제!!
QuizThread클래스를 만들어 스레드를 상속 받는다.
startGame()메서드를 만들고 그 안에서 1 ~ 100사이의 난수 두 개를 더하는 문제를 출제

키보드에서 답을 입력하여 5문제가 정답처리 될 때까지 로직을 반복한다.

정답을 맞히고 난 후에 모든 문제를 맞히는데 몇 초가 걸렸는지를 화면에 출력하며 프로그램 종료.


QuizMain클래스를 만들고 이 메인 클래스에서는

QuizThread qt = new QuizThread();
		qt.start();//스레드 구동
		qt.startGame();//문제풀이 함수

위의 세 줄 이외의 다른 코드는 추가하지 않도록 한다.


단, 사용자가 문제의 정답으로 정수 이외의 문자를 입력했을 경우에
“정답은 정수로 입력하세요”라는 문장이 출력되도록 한다.

---------실행 결과----------- 

23 + 48 = 71
정답!!
66 + 100 = 166
정답!!
68 + 52 = 1
오답
61 + 51 = 112
정답!!
9 + 48 = 57
정답!!
53 + 28 = 81
정답!!
결과 : 24초...
```
#### QuizThread클래스 생성
```java
public class QuizThread extends Thread{

	int su1, su2;
	int timer = 0;
	int playCount = 0;
	boolean isCheck = true;
	final int FINISH = 5;//출제 문제 갯수

	public void startGame(){

		while(isCheck){

			try {
				su1 = new Random().nextInt(100) + 1;
				su2 = new Random().nextInt(100) + 1;
				System.out.print(su1 + " + " + su2 + " = ");
				Scanner scan = new Scanner(System.in);
				int result = scan.nextInt();

				if(result == (su1 + su2)){
					System.out.println("정답!!");
				}else{
					System.out.println("오답");
					continue;
				}	

				playCount++;

				if(playCount == FINISH){
					System.out.println("결과 : " 
							       + timer + "초...");
					isCheck = false;
				}
				
			} catch (Exception e) {
				System.out.println("정답은 정수로 입력하세요");
			}
		}
	}

	@Override
	public void run() {

		while (isCheck) {

			try {
				Thread.sleep(1000);
				timer++;

			} catch (Exception e) {
				// TODO: handle exception
			}			
		}
	}
}
```
#### QuizMain클래스 생성
```java
public class QuizMain {
	public static void main(String[] args) {

		QuizThread qt = new QuizThread();
		qt.start();
		qt.startGame();
		
	}
}
```

#### 스레드 동기화(Synchronized)
휴대폰으로 음악이나 영상을 동기화 하게되면, 동기화가 끝날때까지 휴대폰은 다른작업을 할 수가 없죠?? 그게 이 동기화 스레드의 구조인데요<br>
두 개 이상의 스레드가 하나의 자원을 공유할 경우, 동기화 문제가 발생한다.<br>
변수는 하나인데, 두 개의 스레드가 동시에 한 변수의 값을 변경하다가 오류가 발생할수도 있기 때문입니다.<br>

내가 컴퓨터로 작업을 하다가 잠시 자리를 비운 사이에 누군가가 내 컴퓨터에 손을 대서 내가 하고 있던 작업내용을 바꿔버리지 못하게 하기 위해서, 내 문서작업이 끝날때까지 다른사람이 손대지 못하도록 컴퓨터를 잠궈둘 필요가 있다.<br>

이처럼 특정 스레드들이 공유하는 한 개의 자원을 사용중일 때, 현재 자원을 사용중이지 않은 다른스레드가 작업을 수행하지 못하게 하기 위해 동기화가 필요하다.<br>

#### SynchronizedEx 클래스 정의
```java
public class SynchronizedEx implements Runnable{

	private long money = 10000;//잔액
	//money로 셋터겟터부터 만듦
	//run()정의
	
	@Override
	public void run() {
		
		//synchronized키워드를 사용하면 
		//해당 키워드가 명시되어있는 영역이 마무리 될 때까지
		//다른 스레드에서 접근하지 못하게 된다.
		synchronized (SynchronizedEx.class) { //this
			
			for(int i = 0; i < 10; i++){
				
				try {
					Thread.sleep(500);
				} catch (Exception e) {
					e.printStackTrace();
				}
					
				if(getMoney() <= 0)
					break;//잔액이 0이면 for문을 탈출
				//Main까지 만들어 결과 찍어서 보여준 후에
				//if문 주석달고 son이 구동되는 것도 확인해보자.
//그리고 위의 synchronized (SynchronizedEx.class) 영역도 주석처리해서
//엄마와 아들이 동시에 돈을 찾는 현상도 확인해보자
				
				//outMoney()함수 먼저 정의한 후에 추가
				outMoney(1000);
			}//for
		}//synchronized
	}//run()

	public long getMoney() {
		return money;
	}

	public void setMoney(long money) {
		this.money = money;
	}
	
	public void outMoney(long howMuch){//출금...ㅠㅠ;; 영어모름ㅠ
				
		//해당 스레드의 이름을 가져옴.
		//이름은 해당 스레드를 생성하는 클래스에서 기재하게 됨
		String threadName = Thread.currentThread().getName();
		
		if(getMoney() > 0){//잔액이 0이상일때 출금가능
		   money -= howMuch;//잔액에서 출금액을 마이너스		
		   System.out.println(threadName + " - 잔액 : " + getMoney() + "원");
			
		}else{
			System.out.println(threadName + " - 잔액이 없습니다.");
		}
	}
}
```
#### SyncMain 클래스 정의

```java
public class SyncMain {
	public static void main(String[] args) {
		SynchronizedEx atm = new SynchronizedEx();
		Thread mom = new Thread(atm, "엄마");
		//Thread.currentThread().getName();이 “엄마”가 된다.

		Thread son = new Thread(atm, "아들");
		
		mom.start();
		son.start();
	}
}
```
----------------------------------------------------------------------예제6

#### wait()과 notify()

스레드가 진행중에 wait()을 만나면 일시적으로 정지된다.<br>
스레드가 구동되고 있을 때 일시적으로 대기상태에 보내고, 다른 스레드에게 제어권을 넘긴다.<br>
wait()을 만나 대기상태에 빠진 스레드는 notify()를 만나 재 구동된다.<br>
두 개 이상의 스레드가 구동중일 때 한 개의 동기화 스레드가 작업을 완전히 마칠때까지 기다렸다가 다른 스레드의 작업이 수행되는 것이 아니라 동기화 진행중에 일시적으로 스레드를 정지시키고 다른 스레드가 작업을 할 수 있다.<br>

#### Account클래스 정의
```java
public class Account {
	
	int balance = 1000;//잔액
	
	//출금메서드
	public synchronized void withdraw(int money){
		
		//잔액이 출금액보다 적을 경우
		if(balance < money){
			
			try {
				
				wait();//스레드가 일시적으로 정지상태에 빠짐
				
			} catch (Exception e) {
				// TODO: handle exception
			}						
		}
		
		balance -= money;
	}//withdraw()
	
	//입금메서드
	public synchronized void deposit(int money){
		balance += money;
		notify();//정지된 스레드를 실행
	}
}
```

#### AccountThread클래스 생성

```java
public class AccountThread implements Runnable{

	Account acc;//Account객체 준비
	
	//생성자 정의
	public AccountThread(Account acc) {
		this.acc = acc;
	}
	
	@Override
	public void run() {

		while(true){
			
			try {
				
				Thread.sleep(500);
				
			} catch (Exception e) {
				// TODO: handle exception
			}			
			
			//출금액을 100 ~ 300원 사이의 난수로 지정
			int money = (new Random().nextInt(3) + 1) * 100;
			acc.withdraw(money);
			System.out.println("잔액 : " + acc.balance);
		}
	}
}
```
#### AccountMain클래스 정의
```java
public class AccountMain {
	public static void main(String[] args) {

		Account acc = new Account();

		Runnable r = new AccountThread(acc);
		Thread t1 = new Thread(r);

		t1.start();//스레드 구동

		//스레드와는 별개로 값을받아 입금 시키는
		//코드를 수행할 while()문
		while(true){
			Scanner scan = new Scanner(System.in);
			int n = scan.nextInt();
			acc.deposit(n);
		}
	}
}
```


