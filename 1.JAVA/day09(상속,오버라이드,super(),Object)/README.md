## 상속
api보기 – http://www.oracle.com 접속 – DownLoads마우스 오버 – 왼쪽에 Java for Developers클릭 - 
우측 탭에 Java Resources 아래 APIs – Java SE7클릭
상단의 Tree는 계층도이고, 중간에 See Also는 해당 클래스를 상속받는 객체들이다.
계층도를 보면 Object가 최상위(단군 할아버지)인걸 알 수가 있다.

상속이라 함은 부모가 보유하고 있는 재산 중 일부를 자식이 물려받는 것.<br>
자바라고 하는 프로그래밍 언어에서도 부모자식관계가 존재하고 부모의 재산을 자식이 물려받을수 있도록 설계가 되어 있어요. 상속을 하면서 빼놓을 수 없는 개념인 오버라이드 라고 하는 개념이 있다.<br>

### 패키지 ex_inheritance

#### 1. extends(상속)<br>
클래스 상속은 객체의 재사용이라는 장점뿐 아니라, 코드의 간결성을 제공해주는 객체지향 언어의 최고 장점이라 할 수 있다.<br>
그러므로 잘 정의된 부모클래스(코드 진행하면서 설명)가 있다면 자식 클래스의 작성이 간편해진다는 장점이 있지렁<br>

#### Parent 클래스 정의
```java
public class Parent {
	private int money = 2000000000; //부모의 재산
	String str = "신촌"; //부모의 부동산

변수를 private으로 만들면 자식이 사용할 수 없다.
	
}
```

#### Child클래스 정의(Parent를 상속받는다)

```java
//public class Child{ 현재는 그냥 클래스 이름만 부모 자식인거지 진짜 부모자식은 아님
public class Child extends Parent{

//상속 : 부모가 가지고 있는 재산의 일부를 자식이 물려받는 것.
//Child는 Parent클래스의 자식임을 명시하는 extends키워드를 사용한다.

//자바는 단일상속체제로써 하나의 자식이 여러부모를 동시에 상속(extends)받는 것은 불가능

	String car = "아반떼";	
}
```
#### ExtendsMain클래스 정의

```java
public class ExtendsMain {
	public static void main(String[] args) {

		//Child클래스는 Parent클래스를 상속받은 상태이므로
		//부모로부터 상속받은 money, str등의 변수를 마음대로 가져다가
		//사용할 수 있다.
		Child c1 = new Child();
		
		System.out.println(c1.car); 
		System.out.println(c1.money); 
		System.out.println(c1.str);
    
    //상속관계라고 할지라도 부모클래스는 자식의 재산을 마음대로 가져다가 		
		//사용할 수 없다.
		Parent p1 = new Parent();
		System.out.println(p1.money);
		System.out.println(p1.str);
   
		//c1이 Parent와 상속관계라면
		//instanceof 키워드를 통해서 true값을 얻을 수 있다.


		//c1과 Parent 클래스의 주소가 같습니까?
		if( c1 instancdof Parent) {
			System.out.println(“c1은 Parent의 자식!!”);
		}
		
}

```
부모자식 관계 객체는 자식객체가 메모리 할당이 되면 힙 영역에 놀랍게도 부모 영역이 먼저 만들어진다. 주소: 인스턴스
![image](https://user-images.githubusercontent.com/54658614/220524280-abf49bce-03d4-464c-9953-332e29942db6.png)



자식은 부모의 재산을 쓸수 있지만 부모는 자식의 재산에 손을 대는 것이 불가능하다
![image](https://user-images.githubusercontent.com/54658614/220524449-a00bc57e-8cec-43df-80eb-d126a4807b0b.png)



#### 단일 상속 체제
1) 한명의 자녀가 두명의 부모를 갖는게 불가능 하다.<br>
2) 언제 어떤 상황이 됐는 상속관계의 꼭대기에는 오브젝트가 있다.<br>
오브젝트는 모든 타입을 받아들일 수 있는 최상위 객체다.<br>


----------------------------------------------------------------------예제 1

#### Animal클래스 정의
```java
public class Animal {

	private int eye = 2; //일반적인 동물들의 눈 갯수라고 가정
	private int leg = 4; //일반적인 동물들의 다리 개수라고 가정
	
	public int getEye() {
		return eye;
	}
	public int getLeg() {
		return leg;
	}
	
}
```
#### Cat클래스 정의(Animal상속)
```java
public class Cat extends Animal{
	//고양이은 눈이 2개, 다리가 4개 이므로
	//eye, leg는 부모의 것을 가져다 쓰면 된다
	String balance = "균형감각이 좋다.";
}
```
#### Lion클래스 정의(Animal상속)
```java
public class Lion extends Animal{
	//사자도 눈이 2개, 다리가 4개 이므로
	//eye, leg는 부모의 것을 가져다 쓰면 된다
	String galgi = "탈모걱정 ㄴㄴ";
}
```
#### Snake클래스 정의(Animal상속)
```java
public class Snake extends Animal{

	String sensor = "감각이 발달";
	//오버로드는 메서드의 중복정의
	//메서드 오버라이딩: ‘메서드의 재정의’의 의미를 가진다.
	//상속관계의 객체에서 부모의 메서드를 자식이 가져와 사용하되
	//이름은 그대로 두고, 내용한 현재 자식클래스의 사정에 맞도록 재정의 하는 것.
	//오버라이딩 메서드는 내용을 제외하고는 부모의것과 완전히 동일해야 한다.
	//(접근제한은 부모것 보다 넓은 범위에서 변경 가능)
	@Override
	public int getLeg() {
		return 0;
	}
	
}
```
#### AnimalMain클래스 정의
```java
public class AnimalMain {
	public static void main(String[] args) {
		
		Cat cat = new Cat();
		System.out.println( "---고양이---" );
		System.out.println( "눈 : " + cat.getEye() );
		System.out.println( "다리 : " + cat.getLeg() );
		System.out.println(“특징 : ” + cat.balance());
		
		Lion lion = new Lion();
		System.out.println("---사자---");
		System.out.println("눈 : " + lion.getEye());
		System.out.println("다리 : " + lion.getLeg());
		System.out.println(“특징 : ” + lion.balance());
		
		Snake snake = new Snake();
		System.out.println("---뱀---");
		System.out.println("눈 : " + snake.getEye());
		System.out.println("다리 : " + snake.getLeg());
		
	}//main
```
```java
-----------------------------------------------------------------------예제2

//아주 간단한 오버라이딩 예제를 만들어 봅시다.
	
//Calculator클래스를 만들고
//getResult()함수를 정의하세요. 반환형은 정수.
//인자 두개(n1, n2)를 받고 -1로 리턴하게 만듭니다.
	
//CalPlus클래스를 만들어 Calculator클래스를 상속받으세요.
//오버라이딩을 이용하여 Calculator의 getResult()함수를
//인자로 받은 n1과 n2를 더해주는 함수로 만듭니다.
//물론 리턴값도 -1이면 안되겠죠??
	
//CalMinus클래스를 만들어 Calculator클래스를 상속받으세요.
//오버라이딩을 이용하여 Calculator의 getResult()함수를
//인자로 받은 n1과 n2를 빼주는 함수로 만듭니다.
	
//Main에서 실행하여 아래와 같은 결과가 나오면 성공
//CalPlus : 30
//CalMinus : 15

풀이
Calculator클래스 정의
public class Calculator {
	public int getResult(int n1, int n2){
		return -1;
	}
}

CalPlus클래스 정의
public class CalPlus extends Calculator{
	@Override
	public int getResult(int n1, int n2) {
		// TODO Auto-generated method stub
		return n1 + n2;
	}
}

CalMinus클래스 정의
public class CalMinus extends Calculator{
	@Override
	public int getResult(int n1, int n2) {
		// TODO Auto-generated method stub
		return n1 - n2;
	}
}

CalMain클래스 정의
public class CalMain {
	public static void main(String[] args) {
		CalPlus cp = new CalPlus();
		System.out.println("CalPlus : " + cp.getResult(10, 20));
		
		CalMinus cm = new CalMinus();
		System.out.println("CalMinus : " + cm.getResult(30, 15));
	}
}
```
-----------------------------------------------------------------------예제 3
### super()의 활용

이전에 이야기 했듯이 super는 부모클래스를 의미한다.<br>
super()는 부모클래스의 생성자를 호출하는 것. 간단하게 super()를 알아보자.<br>

#### Parent클래스 정의
```java
public class Parent {
	public Parent() {
		System.out.println("부모(Parent)클래스");
	}//생성자
}
```
![image](https://user-images.githubusercontent.com/54658614/220525863-1dacfa56-1566-4f61-abf3-95609102dc9f.png)

#### Child클래스 정의
```java
public class Child extends Parent{

	//super : 부모 클래스
	public Child() {
		parent(); super(); //부모클래스의 생성자 호출
		System.out.println("자식(Child)클래스");
	}//생성자
}
```
#### SuperMain클래스 정의
```java
public class SuperMain{
	public static void main(String[] args) {
		Child c = new Child();
	}//main
}
```
----------------------------------------------------------------------------
#### Parent클래스에 코드 추가( 파라미터를 받는 생성자)
```java
public class Parent {
	public Parent(int n) {
		System.out.println("부모(Parent)클래스" + n);
	}//생성자
	public int result() {
		return 100;
	}
}
```
#### Child클래스에 코드 추가
```java
public class Child extends Parent{
	public Child() {
		super(1); //부모클래스의 생성자 호출
		System.out.println("자식(Child)클래스");
	}//생성자

	@Override
	public int result() {
		//super.result() : parent클래스의 result()메서드 호출
		return super.result();
	}
}
```
#### SuperMain클래스 주석추가
```java
public class SuperMain {
	public static void main(String[] args) {
		Child ch = new Child();
		//자식클래스를 생성하면
		//자식의 생성자가 호출되는데, 여기서는 자식클래스에서 super()로 
		//부모의 생성자를 먼저 호출했으므로,
		//부모클래스의 생성자에 먼저 접근하게 된다.
	}
}
```
#### 상속과 super()를 이용한 문제
Car 클래스는 gasGauge변수를 갖고 있고, 가스잔여량을 출력하는 함수인 showCurrentGauge()를 갖고 있다.<br>

HybridCar 클래스는 electricGauge변수를 갖고 있고, Car 클래스를 상속하고 생성자를 생성할 때 gasGauge,electricGauge를 파라미터로 받는다.<br>
메서드는 오버라이딩을 이용하여 잔여 가스와 잔여 전기량을 출력<br>
	
HybridWaterCar 클래스는 waterGauge변수를 갖고 있고, HybridCar 클래스를 상속받는다.<br>
생성자 생성할 때는 gasGauge,electricGauge,waterGauge를 파라미터로 받는다.<br>
메서드 오버라이딩을 이용하여 잔여 가스와 잔여 전기량, 잔여 물량 출력<br>
	
main에서 HybridWaterCar 객체를 생성하여 다음과 같은 결과를 출력하시오.<br>

잔여 가스량 : 15<br>
잔여 전기량 : 30<br>
잔여 물량 : 25<br>

#### Car 클래스 생성
```java
class Car {
	private int gasolineGauge; // 변수에 private가 붙어 다른 메소드에서 접근X
	Car(int gasolineGauge) { //생성자 오버로딩으로 파라미터를 받는다.
		this.gasolineGauge = gasolineGauge; 
	}
	// 변수 출력을 위해 해당 출력 메소드를 통해 접근
	public void showCurrentGauge() {
		System.out.println("잔여 가솔린 : " + gasolineGauge);
	
	}
}
```
#### HybridCar 클래스 생성
```java
class HybridCar extends Car	{
	private int electricGauge;
	HybridCar(int gasolineGauge, int electricGauge) {
		super(gasolineGauge);
// 상위 클래스의 변수는 상위 클래스 생성자에서 초기화할 수 있도록 매개변수 전달
		this.electricGauge = electricGauge;	
	}

	public void showCurrentGauge() {
		super.showCurrentGauge();// Car 출력문 호출
		System.out.println("잔여 전기량 : " + electricGauge);
	}
}
```
#### HybridWaterCar 클래스 생성
```java
class HybridWaterCar extends HybridCar {
	private int waterGauge;
	HybridWaterCar(int gasolineGauge, int electricGauge, int waterGauge) {
		super(waterGauge, electricGauge);
		this.waterGauge = waterGauge;
	}
	public void showCurrentGauge() {
		super.showCurrentGauge();
// HybridCar을 호출함으로써 상위 클래스 출력문까지 함께 출력
		System.out.println("잔여 워터량 : " + waterGauge);
	}
}
```
#### CarMain 클래스 생성
```java
class Main {
	public static void main(String[] args) {
		HybridWaterCar hwc = new HybridWaterCar(15, 25, 35);
		hwc.showCurrentGauge();
	}
} 
```
[출처] [JAVA/자바] 상속 오버라이딩 예제|작성자 하늘달<br>

### Object상속관계
#### ExtendsEx1클래스 정의
```java
public class ExtendsEx1 {
	// 현재 클래스의 특정 메서드가 어떤때는 String을 인자로 받고,
	// 어떤때는 int형을 인자로 받는등, 상황에 따라 다른 인자값을 
	// 받아야 한다면....
	// 멤버변수를 어떻게 선언해야 할까?
	// 오버로딩 하는 방법도 있지만 이런 방법도 있다규요

	Object value; //자바 객체의 최상위인 Object형으로 변수생성

	public Object getValue() {
		return value;
	}

	public void setValue(Object value) {
		this.value = value;
	}
}
```
#### ExtendsEx1_Main클래스 정의
```java
public class ExtendsEx1_Main {
	public static void main(String[] args) {

		ExtendsEx1 v1 = new ExtendsEx1();
		v1.setValue("TEST");
		// 인자가 Object이지만 String이 Object를
		// 상속받았으므로 인자로 가능하다.

		System.out.println(v1.getValue()); // TEST

		//이번에는 정수(int)를 인자로 넣어보자!!
		ExtendsEx1 v2 = new ExtendsEx1();
		v2.setValue(100); // AutoBoxing(자동 형변환 : 기본자료형->객체)

		//int i = (Integer)v2.getValue();//Object를 Integer로 형변환 –강제 형변환
		//예전엔 이렇게 객체로 캐스팅하여 썼어야 했다.↑↑↑↑

		//지금은 이렇게 기본자료형으로 캐스팅해도 사용할수 있도록 바뀐 듯
		//UnBoxing – 객체 -> 기본자료형
		int i = (int)v2.getValue();
		System.out.println(i+1);
	}
}
```



