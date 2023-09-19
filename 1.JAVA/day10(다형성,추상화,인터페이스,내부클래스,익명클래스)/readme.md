# 클래스에서의 타입변환
- 타입 변환은 타입을 다른 타입으로 변환하는 것이다.
- 자바에서는 다음과 같이 두 가지의 대표적인 타입 변환이 있다.
    1. 자료형 변환
    2. 클래스의 객체 타입 변환
- 클래스의 타입 변환도 마찬가지로 자동 형 변환과 강제 형 변환이 있다.
- 단, 자료형에 비해 타입 변환이 가능한 범위가 상당이 좁다.
- 클래스의 타입 변환은, 서로 상속 관계에 있는 클래스 사이에서만 변환할 수 있다.

## 클래스의 자동 타입 변환
- 자료형에서의 자동 형 변환과 마찬가지로 개발자가 직접 명시하지 않아도 자동으로 타입 변환이 일어나는 것을 '클래스 자동 타입 변환'이라고 한다.
- 클래스 자동 타입 변환은 상속 관계에 있는 자식 클래스의 객체를 부모 타입의 객체로 변환하는 것을 말한다.
```java
부모클래스명 객체명 = new 자식클래스명();
```
- 이미 만들어진 자식 객체를 부모 타입으로 변환하려고 할 때는 다음과 같이 구현한다.
```java
부모 클래스명 객체명 = 자식객체;
```
## Ex1_class_casting
```java
package test;

class Parent{};

class Child extends Parent{};

public class Test {
	public static void main(String[] args) {
		Parent p1 = new Parent(); //p1 객체 생성
		Child c1 = new Child(); //c1 객체 생성
		
		Parent p2 = new Child(); // 자동 타입 변환
		Parent p3 = c1;			 // 자동 타입 변환
		//Child c2 = p1; 자동 타입 변환이 되지 않음
		
		
		//기본 자료형끼리 비교를 할 때 == 연산자는 값이 같은지 비교하지만
		//객체끼리 비교를 할 때 == 연산자는 주소값이 같은 비교합니다.
		if (p3 == c1) {
			System.out.println("p3와 c1은 같은 객체를 참조하고 있습니다.");
		}
	}
}
```
- 우리는 p3와 c1이 같은 객체를 참조하고 있음을 알 수 있다.
- 즉, Child타입의 Child객체 c1의 타입을 Parent로 변환해 만든 p3는 여전히 c1 객체라는것을 확인할 수 있다.
- 타입을 변환한다고 객체가 바뀌는게 아니라, 객체는 보존되고 사용만 부모 객체처럼 한다.
- 자동 타입 변환은 반드시 자식 클래스의 객체를 부모 타입으로 변환할 때 적용할 수 있다.
- 1차 상속관계가 아니더라도 상위 계층의 타입으로 변환할 수 있다.
- 하지만 같은 상위 계층을 가지고 있더라도, 타입 변환을 시도하려는 두 클래스 간의 상속 관계가 없다면 타입 변환은 불가능하다.

## Ex2_class_casting
```java
package test;

class Car{};
class Bus extends Car{};
class SchoolBus extends Bus{};

class OpenCar extends Car{};
class SportCar extends OpenCar{};

public class Test {
	public static void main(String[] args) {
		Car c1 = new SchoolBus(); //1차 상속 관계가 아니더라도 자동 타입 변환 가능
		Bus b1 = new Bus();
		Bus b2 = new SchoolBus();
		
		Car c2 = new OpenCar();
		OpenCar oc = new SportCar();
		
		//Bus b3 = new OpenCar(); 오류
		//Bus b4 = new SportCar(); d오류
	}
}
```
- 타입을 부모 타입으로 변환한 객체는, 더 이상 자신의 클래스에 부모 클래스와 별개로 추가한 멤버들을 사용할 수 없다.
- 부모 클래스에 선언된 멤버만 사용할 수 있다.
- 단, 부모 클래스의 메서드를 오버라이딩 한 경우 메서드의 경우에는 자식 객체의 것을 호출할 수 있다.

### 자식 객체의 메서드가 호출되는 이유
- 메서드가 실행 시점에서 성격이 결정되는 동적바인딩 때문이다.
- 프로그램의 컴파일 시점에 부모 클래스는 자신의 멤버함수밖에 접근할 수 없으나
- 실행 시점에 동적 바인딩이 일어나 부모가 자식 클래스의 멤버함수를 접근하여 실행할 수 있다.

### 동적바인딩의 작동
1. 클래스 계층구조
    - 자바에서 동적바인딩은 클래스 계층 구조에서 발생한다.
    - 상속하거나 인터페이스를 구현함으로써 계층을 갖는다.
    - 이 계층에서 메서드 오버라이딩이 가능하기 때문이다.
2. 메서드 오버라이딩
    - 자식 클래스는 부모 클래스의 메서드를 재정의(오버라이딩)할 수 있다.
    - 이 때, 자식 클래스에서 부모 클래스의 동일한 이름과 시그니처(함수명,매개변수 개수, 매개변수의 자료형)를 가진 메서드를 재정의한다.
3. 실행시 동적 바인딩
    - 객체가 생성되고 메서드가 호출될 때, 실제로 실행될 메서드는 객체의 실제 타입에 따라 결정된다.
    - 메서드 호출시 객체의 클래스 타입을 기반으로 어떤 메서드를 호출할지 동적으로 결정된다.
  
## Calendar클래스
```java
package test;

public class Calendar {
	String color;
	int months;
	
	public Calendar(String color, int months) {
		this.color = color;
		this.months = months;
	}
	
	public void info() {
		System.out.println(color + " 달력은 " + months + "월까지 있습니다.");
	}
	
	public void hanging() {
		System.out.println(color +"달력을 벽에 걸 수 있습니다.");
	}
}
```
## DeskCalendar클래스
```java
package test;

public class DeskCalendar extends Calendar {
	String color;
	int months;
	public DeskCalendar(String color, int months) {
		super(color,months);
	}
	
	@Override
	public void info() {
		System.out.println(color+"달력을 벽에 걸기 위해 고리가 추가로 필요합니다.");
	}
	
	public void onTheDesk() {
		System.out.println(color+" 달력을 책상에 세울 수 있습니다.");
	}
}
```

## CalendarMain클래스
```java
package test;

public class CalendarMain {

	public static void main(String[] args) {
		DeskCalendar dc = new DeskCalendar("보라색", 6);
		dc.info();
		dc.hanging();
		dc.onTheDesk();
		
		System.out.println();
		
		Calendar c = new DeskCalendar("검은색", 12);
		c.info();
		c.hanging(); //오버라이드된 메서드가 호출된다.
		//c.onTheDesk(); 에러
	}
}
```
- 타입 변환으로 생성된 c 객체는 Calendar타입을 가졌지만, DeskCalendar의 객체이기 때문에 DeskCalendar의 hanging()을 호출했다.
- 클래스 타입 변환을 한 객체는, 더이상 자식 클래스만의 멤버들을 호출할 수 없다.
- DeskCalendar객체임에도 Calendar타입을 가졌기 때문에, DeskCalendar의 멤버에는 접근할 수 없다.

## 클래스의 강제 타입변환
- 위의 코드에서 객체 c처럼 자식 타입에서 부모 타입으로 타입 변환을 했지만 자식 클래스의 멤버에게 접근하고 싶을 때가 생길 수 있다.
- 자바의 규약으로 자식 클래스의 멤버에 접근할 수 없으므로 이러한 경우 다시 DeskCalendar타입으로 변경해서 접근할 수 있도록 해야 한다.
- 우리는 이를 '클래스의 강제 타입 변환'이라고 부른다.
- 자식 객체가 부모 타입으로 자동 타입 변환을 한 후, 다시 자식 타입으로 변환하는 것을 말한다.

```java
일회성으로 타입 변환이 필요할 때는
((자식클래스명)객체명).메서드명();

자식클래스의 멤버에 접근이 여러번 필요한 경우
객체명 = (자식클래스명)부모타입;
```
## Bike클래스
```java
package test;

public class Bike {
	String riderName;
	int wheel = 2;
	
	public Bike(String riderName) {
		this.riderName = riderName;
	}
	
	public void info() {
		System.out.println(riderName + "의 자전거는 " + wheel +"발 자전거입니다.");
	}
	
	public void ride() {
		System.out.println("씽씽");
	}
}
```

## FourWheelBike클래스
```java
package test;

public class FourWheelBike extends Bike{

	public FourWheelBike(String riderName) {
		super(riderName);
		// TODO Auto-generated constructor stub
	}

	@Override
	public void info() {
		// TODO Auto-generated method stub
		super.info();
	}
	
	public void addWheel() {
		if(wheel == 2) {
			wheel = 4;
			System.out.println(riderName+"의 자전거에 보조 바퀴를 부착하였습니다.");
		} else {
			System.out.println(riderName+"의 자전거에 이미 보조 바퀴가 부착되어 있습니다.");
		}
	}
}
```

## BikeMain클래스
```java
package test;

public class BikeMain {
	public static void main(String[] args) {
		Bike b = new FourWheelBike("윤기");
		b.info();
		b.ride();
		//b.addWheel(); 부모 타입으로는 불가
		
		System.out.println();
		
		FourWheelBike fwb = (FourWheelBike)b;
		fwb.addWheel();
		fwb.info();
		fwb.ride();
	}
}
```
- 자식 타입으로 다시 변환을 해줌으로써 부모 타입에서는 사용하지 못했던 자식의 멤버들을 모두 사용할 수 있게 되었다.
- 단, 모든 부모 타입 객체를 자식 타입으로 변환할 수 있는 것은 아니다.
- 반드시 부모 타입으로 자동 타입 변환되었던 자식 객체를 다시 자식 타입으로 변환할 때만 강제 타입 변환을 사용할 수 있다.

# 다형성
- 다형성은 객체 지향 프로그래밍의 대표적인 특징 중 하나로, 하나의 타입으로 다양한 객체를 사용할 수 있다는것을 의미한다.
- 자바에서는 앞에서 학습한 타입 변환을 통해, 부모 클래스의 타입 하나로 여러 가지 자식 객체들을 참조하여 사용함으로써 다형성을 구현할 수 있다.
- 클래스의 타입 변환이 존재하는 이유는 다형성을 구현하기 위함이라고 할 수 있다.
- 완벽한 다형성을 구현하기 위해서는 상속 + 메서드 오버라이딩 + 클래스 타입변환 이 세가지 개념을 합쳐야 한다.
- 객체가 특정 클래스의 필드가 되면서, 하나의 부품처럼 사용될 수 있다.
- 이때, 부품을 교체할 일이 생긴다면 우리는 다형성을 구현함으로써 코드 수정을 최소화할 수 있다.

## Computer클래스
```java
package test;

public class Computer {
	public void powerOn() {
		System.out.println("삑- 컴퓨터가 켜졌습니다.");
	}
	
	public void powerOff() {
		System.out.println("컴퓨터가 종료됩니다.");
	}
}
```

## Samsung클래스
```java
package test;

public class Samsung extends Computer{
	@Override
	public void powerOn() {
		super.powerOn();
		System.out.println("아이 러브 삼송");
	}
	
}
```
## ComputerRoom클래스
```java
package test;

public class ComputerRoom {
	Samsung com1;
	Samsung com2;
	
	public void allPowerOn() {
		com1.powerOn();
		com2.powerOn();
	}
	
	public void allPowerOff() {
		com1.powerOff();
		com2.powerOff();
	}
}
```

## ComMain클래스
```java
package test;

public class ComMain {

	public static void main(String[] args) {
		ComputerRoom cr = new ComputerRoom();
		cr.com1 = new Samsung();
		cr.com2 = new Samsung();
		
		cr.allPowerOn();
		cr.allPowerOff();
		
		System.out.println();

	}
}
```
- 지금까지는 사용하는데 아무런 불편함이 없다.
- 하지만 컴퓨터실에 있는 컴퓨터 두대를 LZ컴퓨터로 바꾸고 싶다면 어떻게 해야할까??

## LZ클래스
```java
package test;

public class LZ extends Computer{
	
	@Override
	public void powerOn() {
		super.powerOn();
		System.out.println("사랑해요 LZ");
	}
}
```

## ComputerRoom 클래스 수정하기
```java
package test;

public class ComputerRoom {
	//Samsung com1;
	//Samsung com2;
	
	//LZ com1;
	//LZ com2;

	//매번 다른 브랜드의 컴퓨터로 바꾸기 위해 코드를 수정하면 피곤해지기도 하고
	//실수를 할 위험성도 커진다.

	//클래스의 타입변환을 사용하면 보다 간결하게 해결할 수 있다.
	Computer com1;
	Computer com2;
	
	public void allPowerOn() {
		com1.powerOn();
		com2.powerOn();
	}
	
	public void allPowerOff() {
		com1.powerOff();
		com2.powerOff();
	}
}

```

## ComMain클래스 수정하기
```java
package test;

public class ComMain {

	public static void main(String[] args) {
		ComputerRoom cr = new ComputerRoom();
		//cr.com1 = new Samsung();
		//cr.com2 = new Samsung();
		cr.com1 = new LZ();
		cr.com2 = new LZ();
		
		cr.allPowerOn();
		cr.allPowerOff();
		
		System.out.println();

	}
}
```

## instancdof연산자
- 부모 타입으로 타입이 변환되어 저장된 변수는 안에 어떤 객체가 담겨 있는지 직접 확인하지 않는 이상 내부 객체를 알기 쉽지 않다.
- 오버라이딩된 메서드가 있다면 확인이 쉽겠지만, 부모클래스를 같이 상속받고 있는 다른 클래스 또는 부모클래스와 구별할 수 있는 특정 멤버가 없다면 어떻게 구별해야 할까?
- instanceof연산자의 특징
    -  A instanceof B : A 객체가 생성될 때 B 타입으로 생성되었는지 확인하는 연산자
    -  맞으면 true, 아니면 false를 반환하며 만약 null을 가리키고 있으면 false를 반환한다.

## FarmTest클래스
```java
package test;

class Animal{}
class Pig extends Animal{}
class Cow extends Animal{}

class Farm{
	public void sound(Animal animal) {
		if(animal instanceof Pig) {
			System.out.println("꿀꿀");
		} else {
			System.out.println("음메");
		}
	}
}


public class FarmTest {
	public static void main(String[] args) {
		Farm f = new Farm();
		Pig p = new Pig();
		Cow c = new Cow();
		f.sound(p);
		f.sound(c);
	}
}
```

## 오버라이딩으로 해결하기
- instanceof 연산자를 사용하지 않고도, 오버라이딩을 사용해 같은 문제를 해결할 수 있다.
```java
package test;

class Animal{
	public void cry() {};
}
class Pig extends Animal{
	@Override
	public void cry() {
		System.out.println("꿀꿀");
	}
}
class Cow extends Animal{
	@Override
	public void cry() {
		System.out.println("꿀꿀");
	}
}

class Farm{
	public void sound(Animal animal) {
		animal.cry();
	}
}


public class FarmTest {
	public static void main(String[] args) {
		Farm f = new Farm();
		Pig p = new Pig();
		Cow c = new Cow();
		f.sound(p);
		f.sound(c);
	}
}
```
# 추상화
- 바로 전에 작성했던 FarmTest에서 메서드 오버라이딩을 이용하여 처리한 경우를 다시한번 살펴보자.
- Animal클래스의 cry()메서드가 텅 비어있는 것을 확인할 수 있다.
- Animal 객체를 통해 직접 cry()메서드를 호출할 일은 없지만,
- Animal클래스를 상속받은 자식 클래스들이 cry()메서드를 오버라이딩 하여 재정의 하고
- 타입변환을 통해서 그 메서드를 사용하기 위함이었다.

## 추상메서드
- 선번부만 작성하고 구현부는 작성하지 않고 남겨둔 미완성 메서드를 '추상 메서드'라고 한다.
- 다형성을 위해 메서드의 선언은 통일해야 하지만, 실제로 구현하는 내용은 자식클래스마다 달라야 할 때
- 부모 클래스의 메서드는 비워두고 자식 클래스에서 오버라이딩하여 구현을 할 수 있다.
- 추상 메서드를 선언할 때 abstract 키워드를 함께 표기해야 한다.
- 또한 메서드의 구현부인 중괄호{} 대신 구현부가 없다는 의미로 세미콜론(;)를 쓴다.

```java
[접근제한] abstract [반환형] [메서드명](파라미터1,파라미터2);
abstract [접근제한] [반환형] [메서드명](파라미터1,파라미터2);
```
## 추상 클래스
- 추상메서드가 한 개 이상 정의되어 있는 클래스를 추상 클래스라고 한다.
- 추상 메서드를 포함하고 있다는 것을 제외하고 일반 클래스와 다르지 않다.
- 추상 클래스에도 생성자가 있으며, 멤버변수와 메서드도 가질 수 있다.
- 추상 클래스 또한 abstract를 통해 자신이 추상클래스임을 명시해줘야 한다.
```java
[접근제한] abstract class [클래스명]{
	//필드
	//생성자
	//메서드(추상메서드 포함)
}
```
### 추상 클래스의 특징
- 일반 클래스 처럼 독립적으로 생성자를 호출해 객체를 생성할 수 없다.
- 자식 클래스의 생성자에 super()를 총해 추상 클래스의 생성자를 호출하여 부모 객체를 생성한 후 자식 객체를 생성한다.

## AbsParent클래스(abstract)
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

## AbsChild클래스 정의
```java
public class AbsChild extends AbsParent{
	
	//추상 클래스를 상속받은 자식 클래스는
	//부모가 가지고 있는 추상 메서드(미완성)를 무조건 받아두어야 한다.
	//재정의 할 필요는 없지만 오버라이딩 해서 가지고는 있어야 한다는 의미.

	//추상클래스에서 만든 메서드를 몸체까지 강제로 오버라이딩이 됨.
	//무조건 받아야 하고 자식클래스의 상황에 맞게 내용을 정의할 수 있다.

	@Override
	public void setValue(int n) {
		System.out.println("추상메서드 재정의함");
	}
```	
	
## AbsMain정의
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

## AbsClass정의
```java
public abstract class AbsClass {
	int value = 100;

	public int getValue(){
		return value;
	}

	public abstract int changeValue(); // 추상메서드
}
```

## Phone 클래스
```java
package test2;

public abstract class Phone {
	abstract public void openingLogo();
	
	public void powerOn() {
		openingLogo();
		System.out.println("핸드폰이 켜집니다.");
	}
	
	public void powerOff() {
		System.out.println("핸드폰이 꺼집니다.");
	}
}
```

## PineapplePhone클래스
```java
package test2;

public class PineApplePhone extends Phone{

	@Override
	public void openingLogo() {
		System.out.println("@@@");
	}
}
```

## ThreeStarPhone클래스
```java
package test2;

public class ThreeStarPhone extends Phone{

	@Override
	public void openingLogo() {
		System.out.println("★★★");
	}
}
```

## PhoneMain클래스
```java
package test2;

public class PhoneMain {
	public static void main(String[] args) {
		PineApplePhone pp = new PineApplePhone();
		pp.powerOn();
		pp.powerOff();
		
		System.out.println();
		
		ThreeStarPhone tp = new ThreeStarPhone();
		
		tp.powerOn();
		tp.powerOff();
	}
}
```

# 인터페이스
- 모든 메서드가 추상 메서드인 일종의 추상 클래스를 '인터페이스'라고 부른다.
- 인터페이스는 추상 메서드와 상수로만 이루어져 있으며, 추상클래스와 마찬가지로 스스로 객체를 생성할 수 없다.
- 언뜻 보면 인터페이스와 추상 클래스가 같은 역할을 하는 것처럼 느껴질 수 있지만, 취지는 완전히 다르다.
- 추상 클래스는 자식클래스들의 공통적인 특징을 추출하고 제공하는것이 주된 역할이었다면
- 인터페이스는 그뿐 아니라 다른 클래스 코드들과의 중간 매개 역할을 하는 것을 중점으로 생각할 수 있다.

## 인터페이스의 선언
- 인터페이스는 클래스가 아니다.
- 추상클래스는 스스로 객체를 생성할 수는 없지만, 자식 클래스의 생성자를 통해 객체를 생성해낼 수 있었다.
- 하지만 인터페이스는 어떤 형태로도 객체를 만들 수 없기 때문에 클래스라고 부를 수 없다.
- 인터페이스는 객체의 매개체, 즉, 객체를 사용하는 방법을 제공하는 새로운 블록이라고 할 수 있다.

```java
[접근제한자]interface 인터페이스명{
	//상수
	//추상메서드
}
```

## Phone클래스
```java
package test3;

public interface Phone {
	
	public static final int MAX_BATTERY_CAPACITY = 100;
	
	abstract void powerOn();
	abstract void powerOff();
	abstract boolean isOn();
	abstract void watchUtube();
	abstract void charge();
}
```
- 추상 클래스는 추상 메서드가 비어있기 때문에 객체 생성을 스스로 할 수 없다.
- 대신 자식 클래스의 생성자의 힘을 빌려 객체 생성을 할 수 있었다.
- 인터페이스도 마찬가지로 추상 메서드가 비어있기 때문에 객체 생성을 스스로 할 수 없다.
- 따라서 인터페이스도 자신이 가지고 있는 추상 메서드를 구현해줄 클래스를 작성해야만 한다.
- 인터페이스를 구현해주는 클래스를 '구현 클래스'라고 한다.

### implements
- 구현 클래스는 인터페이스를 사용해 구현하겠다는 선언을 해야 한다.
- 구현한다는 의미를 가지고 있는 implements키워드를 사용하여 명시할 수 있다.
```java
[접근제한자]class 클래스명 implements 인터페이스명{
	//필드
	//생성자
	//메서드(추상메서드 오버라이딩)
}
```

## PineApplePhone클래스
```java
package test3;

public class PineApplePhone implements Phone{
	int batteryCapacity = 40;
	boolean isOn = false;
	
	@Override
	public void powerOn() {
		if(batteryCapacity > 30) {
			System.out.println("@@@Power On!!@@@");
			isOn = true;
		} else {
			System.out.println("Low Battery...");
		}
		
	}
	
	@Override
	public void powerOff() {
		System.out.println("@@@Power Off!!@@@\n");
		isOn = false;
		
	}
	
	@Override
	public boolean isOn() {
		if(isOn) {
			return true;
		} else {
			return false;
		}
	}
	
	@Override
	public void watchUtube() {
		if(batteryCapacity > 10) {
			System.out.println("--- watching Utube ---");
			batteryCapacity -= 10;
			System.out.println("battery is..." + batteryCapacity + "%\n");
		} else {
			System.out.println("Low Battery...");
			powerOff();
		}
		
	}
	
	@Override
	public void charge() {
		if(batteryCapacity < Phone.MAX_BATTERY_CAPACITY - 20) {
			System.out.println("--- charging ---");
			batteryCapacity += 5;
			System.out.println("Charged..." + batteryCapacity + "%\n");
		} else {
			System.out.println("You don't have to charge...");
			System.out.println("It's enough... " + batteryCapacity + "%");
		}
		
	}
}
```

## ThreeStarPhone클래스
```java
package test3;

public class ThreeStarPhone implements Phone{
	int batteryCapacity = 40;
	boolean isOn = false;
	
	@Override
	public void powerOn() {
		if(batteryCapacity > 30) {
			System.out.println("★★★폰이켜졌습니다.★★★");
			isOn = true;
		} else {
			System.out.println("배터리가 부족합니다...");
		}
		
	}
	
	@Override
	public void powerOff() {
		System.out.println("★★★폰이 꺼졌습니다.★★★\n");
		isOn = false;
		
	}
	
	@Override
	public boolean isOn() {
		if(isOn) {
			return true;
		} else {
			return false;
		}
	}
	
	@Override
	public void watchUtube() {
		if(batteryCapacity > 10) {
			System.out.println("--- U튜브 시청 중 ---");
			batteryCapacity -= 10;
			System.out.println("잔여 배터리" + batteryCapacity + "%\n");
		} else {
			System.out.println("배터리가 부족합니다...");
			powerOff();
		}
		
	}
	
	@Override
	public void charge() {
		if(batteryCapacity < Phone.MAX_BATTERY_CAPACITY - 20) {
			System.out.println("--- 충전중 ---");
			batteryCapacity += 5;
			System.out.println("잔여 배터리" + batteryCapacity + "%\n");
		} else {
			System.out.println("충전할 필요가 없습니다.");
			System.out.println("잔여 배터리..." + batteryCapacity + "%");
		}
		
	}
}
```

## Person클래스
```java
package test3;

public class Person {
	Phone p;
	
	public Person(Phone p) {
		this.p = p;
	}
	
	public void buyNewPhone(Phone p) {
		this.p = p;
		System.out.println(" = = = = = = == =");
		System.out.println("새 폰을 샀습니다.");
	}
	
	public void turnOnPhone() {
		p.powerOn();
	}
	
	public void turnOffPhone() {
		p.powerOff();
	}
	
	public void watchUtube() {
		if(p.isOn()) {
			p.watchUtube();
		}else {
			System.out.println("폰이 꺼져 있기 대문에 U튜브를 볼 수 없습니다.");
		}
	}
	
	public void chargePhone() {
		p.charge();
	}
}
```

## PhoneMain클래스
```java
package test3;

public class PhoneMain {
	public static void main(String[] args) {
		Person jimin = new Person(new PineApplePhone());
		jimin.turnOnPhone();
		for(int i = 1; i < 6; i++) {
			jimin.watchUtube();
			
			if(i % 3 == 0) {
				jimin.chargePhone();
			}
		}
		
		jimin.buyNewPhone(new ThreeStarPhone());
		jimin.turnOnPhone();
		
		for(int i = 1; i < 5; i++) {
			jimin.watchUtube();
			
			if(i % 3 == 0) {
				jimin.chargePhone();
			}
		}
	}
}
```
### 인터페이스의 장점
- 정보은닉 : 실제 구현 클래스의 내용을 전혀 보지 않고도 개발 코드로 객체를 사용할 수 있다.
- 모듈화 : 구현 클래스들이 독립적으로 구현되고 사용될 수 있다. 개발 코드에서 객체 변경이 필요할 때, 개발코드의 수정을 최소화할 수 있다.

## 다중 인터페이스 구현
- 우리는 하나의 클래스로 여러 개의 인터페이스를 구현할 수 있다.
- 선언한 모든 인터페이스에 대한 추상 메서드를 모두 구현해 줘야 한다.
```java
[접근제한자]class 클래스명 implements 인터페이스1,인터페이스2{
	//필드
	//생성자
	//인터페이스1에 대한 구현 메서드
	//인터페이스2에 대한 구현 메서드
}
```
## Menu1 인터페이스 정의
```java
public interface Menu1 {
	abstract String jajang();

	//abstract는 생략되어도 interface안에서는 자동으로 추상으로 인식.
	String jjambbong();
}
```

## Menu2 인터페이스 정의
```java
public interface Menu2 {
	abstract String tangsuyuck();//탕수육 철자 모르겠다;;ㅋ
}
```

## Menu3 인터페이스 정의
```java
public interface Menu3 extends Menu1, Menu2{
	//인터페이스는 일반클래스 상속이 불가능하고 인터페이스만 상속이 가능하다.
	//인터페이스는 구현능력이 없기 때문에 다중상속이 가능하다
	abstract String boggembab();//보끔밥....
}
```

## Kitchen 클래스 생성
```java
public class Kitchen implements Menu1, /*Menu2, Menu3*/ {
	@Override
	public String jajang() {
		// TODO Auto-generated method stub
		return "중면 + 춘장 + 완두콩";
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

## InterMain 클래스 정의
```java
package test3;

public class KitchenMain {

	public static void main(String[] args) {
		Kitchen k = new Kitchen();
		
		Menu1 im1 = k;
		Menu2 im2 = k;
		Menu3 im3 = k;
		
		//타입 변환된 각 인터페이스 내에 정의된 메서드들로 국한된다.
		
		//im1 객체에서는 jjambbong()과 jajang()만 호출할 수 있다.
		System.out.println(im1.jajang());
		System.out.println(im1.jjambbong());
		
		//im2 객체에서는 tangsuyuck()만 호출할 수 있다.
		System.out.println(im2.tangsuyuck());
		
		//im3객체에서는 boggembab()만 호출할 수 있다.
		System.out.println(im3.boggembab());

	}

}
```

# 내부 클래스
- 내부 클래스는 클래스 안에 만들어진 또 다른 클래스로 중첩 클래스라고도 부른다.
- 클래스에 다른 클래스를 선언하는 이유는 두 개의 클래스가 서로 긴밀한 관계를 맺고 있기 때문이다.

## 내부 클래스의 장점
- 두 클래스 멤버들 간에 손쉽게 접근할 수 있다.
- 불필요한 클래스를 감춰서 코드의 복잡성을 줄일 수 있다.

```java
public class OuterClass{ //외부 클래스

	class InnerClass{ //내부 클래스

	}
}
```

## 내부 클래스의 종류
|메서드|설명|
|-----|-----|
|인스턴스 클래스|외부 클래스의 멤버 변수와 같은 위치에 선언<br>주로 외부 클래스의 멤버 변수와 관련된 작업에 사용될 목적으로 선언|
|정적 클래스|외부 클래스의 클래스 변수와 같이 static키워드 부여|
|지역 클래스|외부 클래스의 메서드 내부에서 선언하여 사용<br>메서드 영역에서 선언되기 때문에 메서드 내부에서만 사용 가능|

## 인스턴스 클래스
- 인스턴스 클래스는 외부 클래스 내부에서 생성하고, 선언되어 사용하는 클래스를 의미한다.
- 인스턴스 변수(필드)와 같은 위치에 선언하며, 외부 클래스의 인스턴스 멤버처럼 다루어진다.
- 주로 외부클래스의 멤버들과  관련된 작업에 사용될 목적으로 선언된다.

```java
public class Outer{
	private String name; //필드

	//인스턴스 클래스
	public class Inner{
		private String name;
	}

}
```
- 내부 클래스도 외부 클래스 안에 생성되는 것 외에는 별도의 클래스이기 때문에, 파일이 컴파일되면 별도로 생성된다.

## 인스턴스 클래스의 객체화
- 인스턴스 클래스는 기본적인 내부 클래스이다.
- 외부 클래스 안에 생성되기 때문에, 클래스를 사용하려면 외부 클래스객체가 생성된 상태에서 객체 생성을 할 수 있다.
```java
Outer outer = new Outer(); //외부 클래스 객체 생성
Outer.Inner in = Outer.new Inner(); //외부 클래스를 이용해 내부 클래스 객체 생성
```

## CalculatorExample클래스
```java
package test3;

class Calculator{
	private int val1;
	private int val2;
	
	public Calculator(int val1, int val2) {
		this.val1 = val1;
		this.val2 = val2;
	}
	
	public class Calc{
		public int add() {
			return val1 + val2;
		}
	}
}


public class CalculatorExample {
	public static void main(String[] args) {
		Calculator cal = new Calculator(10, 11);
		Calculator.Calc c = cal.new Calc();
		System.out.println("합 : " + c.add());
	}
}
```
- 하나의 소스파일에 둘 이상의 public class가 존재하면 안된다.
- 각 클래스를 별도의 소스파일에 나눠서 저장하던가 아니면 둘 중 한 클래스에만 public을 붙혀야 한다.

## 정적 내부 클래스(static class)
- 클래스 안에 정적 변수를 선언할 수 있는 것처럼 클래스도 정적 클래스를 만들 수 있다.
- 필드(멤버)와 마찬가지로 static키워드를 사용해 클래스를 선언한 후 정적 내부 클래스를 생성한다.
- 주로 외부 클래스의 static메서드에서 사용될 목적으로 만든다.

```java
public class Outer{
	private String name; -> 필드

	public static class Inner{
		private String name;
	}
}
```
- 외부 클래스의 필드 또는 메서드를 정적 내부 클래스 안에서는 사용할 수 없다.
```java
public class Outer{
	private int val1; //필드

	public static class Inner{
		public void add(){
			int result = val1 + 10; -> 에러
		}
		
	}
}
```
- 정적 내부 클래스는 정적 변수 또는 정적 메서드를 호출하는 것은 가능하다.
```java
public class Outer{
	private int val1; //필드
	private static int cnt = 1; //클래스 변수

	public static class Inner{
		public void displayOuterInfo(){
			System.out.println(val); -> 에러
			System.out.println(cnt);
		}
		
	}
}
```

## 정적 내부 클래스의 객체 생성
- 외부 클래스의 객체를 생성하지 않아도 선언할 수 있다.
```java
Outer.Inner in = new Outer.Inner();
```

## StaticClassExam클래스
```java
package test3;

class PrintOut{
	public static class Out{
		public void println(String str) {
			System.out.println(str);
		}
	}
}

public class StaticClassExample {
	public static void main(String[] args) {
		String str = "정적 내부 클래스 테스트";
		PrintOut.Out out = new PrintOut.Out();
		out.println(str);
	}
}
```

## 지역클래스(local class)
- 지역 클래스는 외부 클래스의 메서드 내에서 선언되어 사용하는 클래스이다.
- 메서드 내에서 선언되기 때문에 해당 클래스는 메서드 내에서만 사용할 수 있다.
- 메서드의 실행이 끝나면 해당 클래스도 사용이 종료된다.
```java
public class LocalClass{
	public void print(){
		///////////////////
		class A{
				지역 클래스 선언
		}

		A a = new A();	메서드 내에서 사용
		//////////////////

	}

}
```
## LocalClassExample클래스
```java
package test3;

public class LocalClassExample {
	private int speed = 10;
	
	public void getUnit(String unitName) {
		
		class Unit{
			public void move() {
				System.out.println(unitName + "이 " + speed + " 속도로 이동합니다.");
			}
		}
		
		Unit unit = new Unit();
		unit.move();
	}
	
	public static void main(String[] args) {
		LocalClassExample local = new LocalClassExample();
		local.getUnit("마린");
	}
}
```

## 내부 클래스의 접근 제한
- 내부 클래스도 클래스이기 때문에 접근 제한자를 붙여서 사용할 수 있다.

## Permit클래스
```java
package test3;

public class Permit {
	private class InClass{
		public void print() {
			System.out.println("접근을 private로 제한합니다.");
		}
	}
	
	public void getInClass() {
		InClass inClass = new InClass();
		inClass.print();
	}
}
```

## PermitMain클래스
```java
package test3;

public class PermitMain {

	public static void main(String[] args) {
		Permit permit = new Permit();
		
		permit.getInClass();

	}
}
```

## 지역 클래스의 접근 제한
- 지역 클래스는 메서드 내에서 선언되어 사용한다.
- 보통 메서드가 종료되면 클래스도 함께 종료되지만 메서드와 실행되는 위치가 다르기 때문에 종료되지 않고 남아있을 수도 있다.
- 그래서 지역 클래스에서 메서드 내의 변수를 사용할 때는 변수를 복사해 사용한다.
- 이러한 이유로 지역 클래스에서 메서드의 변수를 사용할 때 해당 변수가 변경되면 오류가 발생한다.

## LoacalClassExample클래스 코드 추가하기
```java
package test3;

public class LocalClassExample {
	private int speed = 10;
	
	public void getUnit(String unitName) {
		unitName = unitName + " 님";
		
		class Unit{
			public void move() {
				//unitName에서 에러 발생
				System.out.println(unitName + "이 " + speed + " 속도로 이동합니다.");
			}
		}
		
		Unit unit = new Unit();
		unit.move();
	}
	
	public static void main(String[] args) {
		LocalClassExample local = new LocalClassExample();
		local.getUnit("마린");
	}
}

```
- 자바 7버전까지는 지역 클래스에서 메서드의 변수를 사용하려면 final키워드를 붙여서 사용하도록 했으나
- 자바 8버전부터는 해당 변수를 변경하지 않는다는 조건하에 effective final이라는 기능이 추가되어 키워드를 사용하지 않아도 final변수로 인정된다.

## 익명클래스(anonymous class)
- 다른 내부 클래스와는 달리 이름이 없는 클래스를 의미한다.
- 익명 클래스는 클래스의 선언과 객체의 생성을 동시에 하므로 한 번만 사용할 수 있다.
- 오직 하나의 객체만을 생성할 수 있는 일회용 클래스이다.
- 따라서 생성자를 선언할 수도 없으며, 둘 이상의 인터페이스를 구현할 수도 없다.

## 보통의 상속관계 처리
- Person클래스
```java
package test3;

public class Person {
	public void mySelf() {
		System.out.println("나는 인간입니다.");
	}
}
```
- Student클래스
```java
package test3;

public class Student extends Person{
	@Override
	public void mySelf() {
		System.out.println("I'm child");
	}
}
```
- PersonMain
```java
package test3;

public class PersonMain {
	public static void main(String[] args) {
		Student s = new Student();
		s.mySelf();
	}
}
```
- Person클래스를 확장하기 위해 자식 클래스를 만들어서 사용한다.
- 그런데 만약 Person을 상속받아 처리해야 하는 클래스가 또 필요하지만 한번만 사용할 거라면 굳이 상속할 필요가 없다.

```java
package test3;

public class PersonMain {
	public static void main(String[] args) {
		Student s = new Student();
		s.mySelf();
		
		Person p2 = new Person() {
			@Override
			public void mySelf() {
				System.out.println("저는 직장인입니다.");
			}
		};
		
		p2.mySelf();
	}
}
```
- 생성자 뒤에 코드 블록{}이 추가되고, 해당 클래스가 가진 메서드들을 override하여 구현하는 방법이다.
- 해당 클래스 자체를 재정의하여 구현한다.
- 구현된 문법 마지막에는 세미콜론(;)을 붙힌다.
- 익명 클래스는 보통 인터페이스의 기능을 구현할 때 사용한다.
- 인터페이스를 상속하여 하위 클래스를 통해 구현하는 것이 아니라 인터페이스를 익명 클래스로 선언하여 기능을 직접 구현한다.

## AnonymousExample클래스
```java
package test;

interface buttonClickListener{
	public void click();
}

public class AnonymousExample {
	
	public class Button{
		private buttonClickListener listener;
		
		public void setButtonListener(buttonClickListener listener) {
			this.listener = listener;
		}
		
		//버튼 클릭 기능
		public void click() {
			if(listener != null) {
				this.listener.click();
			}
		}
	}
	
	public static void main(String[] args) {
		AnonymousExample exam = new AnonymousExample();
		AnonymousExample.Button button = exam.new Button();
		
		button.setButtonListener(new buttonClickListener() {
			
			@Override
			public void click() {
				System.out.println("버튼을 눌렀습니다.");
			}
		});
	}
}
```

### 열거형(enum)
열거형은 상수를 가지고 생성되는 객체들을 한곳에 모아둔 하나의 묶음이다.<br>
변수를 사용자가 지정한 이름으로 0부터 순차적으로 증가시켜준다.<br>



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
			System.out.println("메인함수 실행중" + I);
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

	System.out.println("현재실행되고있는스레드의이름: Thread.currentThread().getName());
	System.out.println("현재실행되고있는스레드의상태: Thread.currentThread().getState());
	System.out.println("현재실행되고있는스레드의우선순위: Thread.currentThread().getPriority);
}
```

#### ThreadName 클래스 생성

```java
class ThreadName extends Thread {
	@Override	
	public void run() {
		this.setName("Thread3");
		System.out.println("현재실행되고있는스레드의이름: Thread.currentThread().getName());
	System.out.println("현재실행되고있는스레드의상태: Thread.currentThread().getState());
	System.out.println("현재실행되고있는스레드의우선순위: Thread.currentThread().getPriority);
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
		System.out.println("카운트다운 5초");
		for(int I = 5; i>=0; I--) {
			if(i!=0) {
				try{
					Thread.sleep(1000);//1000당 1초
				} catch(Exception e) {

				}//catch
			}//if
		}//for
		System.out.println("종료!");
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
			System.out.println("★");
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
			System.out.println("☆");
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
			System.out.println("t1:" + i);
			
		}
		System.out.println("<<t1완료>>");
	}
}

#### JoinTest2 클래스 생성
```java
public class JoinTest2 implements Runnable{
	public void run() {
		for(int I = 0; i<10; I++) {
			System.out.println("t2:" + i);
		}
		System.out.println("<<t2완료>>");
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
			System.out.println("메인스레드:" +i);
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
"종료"라는 메시지와 함께 스레드를 빠져나오도록 만들어보자.

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
"정답은 정수로 입력하세요"라는 문장이 출력되도록 한다.

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
		//Thread.currentThread().getName();이 "엄마"가 된다.

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


