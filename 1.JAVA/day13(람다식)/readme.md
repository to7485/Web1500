# 람다식
- JDK1.8부터 함수형 프로그래밍 '람다식(Lambda expression)'을 지원하고 있다.
- 람다식은 이름이 없는 익명 함수(anonymous function)를 만들기 위한 표현식을 말한다.
- 자바는 객체를 기반으로 프로그램을 구현하는 객체 지향 프로그램이다.
- 따라서 클래스를 먼저 생성하고, 클래스 안에 메서드와 객체를 만들어 사용해야 한다.
- 하지만 함수형 프로그래밍은 객체 지향 프로그램과 달리 함수만을 구현하고 실행할 수 있는 개발방식이다

### 자바스크립트의 예시 인터넷창 개발자도구 -> console탭에서 작성하기
```java
function add(num1,num2){
	return num1+num2;
}

add(10,20)
30
```
![image](https://user-images.githubusercontent.com/54658614/224224980-44b27fa7-25b3-4d2d-95ec-ad3a425798c7.png)

## 람다식이 도입된 이유
- 함수형 프로그래밍 방식
- 자바에서는 함수형 프로그래밍 방식이 적용되지 않았다.
- 자바 공부할 때 함수는 어디에 만들었는가 클래스를 나누어서 작성했었다.
- 자바 -> 함수가 독립적이지 않다. -> 반드시 객체를 만들어서 호출

## 람다식 문법
- 람다식 문법은 기존의 자바 문법과는 달라서 객체 지향 프로그래밍에 익숙한 개발자들은 다소 생소할 수 있다.
- 하지만 문법이 매우 간결해지고, 원하는 결과를 쉽게 집계할 수 있어 익숙해지면 큰 장점이 있다.

```java
int add(int x, int y){
	return x+y;
}
```
- 위의 두 수를 더하는 메서드를 람다식으로 표현해보자
```java
  (x,y) -> {return x + y'}
매개변수	함수 구현
```
- 메서드의 이름과 반환 타입을 제거하고 화살표(->)를 이용해 구현한다.
- 의미를 살펴보면 두 개의 파라미터(x,y)를 사용해 더한 결과를 반환하라는 의미이다.

### 소괄호 생략하기
- 람다식 문법에서는 파라미터의 자료형을 생략할 수 있다.
- 파라미터가 한 개인 경우에는 소괄호도 생략할 수 있다.
- 그러나 파라미터가 두 개 이상일 경우에는 생략할 수 없다.
```java
(str) -> {System.out.println(str);}
	↓ 파라미터가 1개일 때
str -> {System.out.println(str);} //소괄호 생략 가능

(x,y) -> {return x+y;}
	↓ 파라미터가 2개 이상일 때
x,y -> {return x+y;} //오류! 소괄호 생략 불가
```

### 중괄호 생략하기
- 함수의 반환형이 void라면 중괄호도 생략할 수 있다.
```java
(x,y) -> {System.outprintln(x+y);}
	↓ return이 없는 경우
(x,y) -> System.out.println(x+y); //중괄호 생략 가능
```

### return 생략하기
- 중괄호 안의 구현 코드가 return문만 존재할 때는 중괄호와 return을 모두 생략할 수 있다.
```java
(x,y) -> {return x+y;}
	↓ return문장만 있는 경우
(x,y) -> x+y;
```

## 함수형 인터페이스
- 객체 지향 프로그램에서 인터페이스를 사용하려면 인터페이스를 클래스에서 구현한뒤 사용해야 한다.
- 람다식은 위와 같은 과정을 생략할 수 있다.
- 단, 람다식을 이용해 인터페이스를 사용할 경우, 인터페이스는 하나의 기능만을 정의할 수 있다.
- 람다식을 구현하기 위해서는 먼저 인터페이스를 만들고, 인터페이스에 람다식으로 구현할 메서드를 선언해야 한다.
- 오직 하나의 추상 메서드가 선언된 인터페이스만이 람다식의 타겟 타입이 될 수 있는데
- 이러한 인터페이스를 '함수형 인터페이스'라고 한다.

### MyCalculator 인터페이스 생성하기
```java
package calculator;

public interface MyCalculator {
	int plus(int num1,int num2);
}
```
- 간혹 프로그래밍을 하다보면 람다식으로 구현한 인터페이스에 실스로 두 개 이상의 메서드를 추가하는 오류를 범할 수 있다.
- 이를 방지하고자 어노테이션을 부여해 제한을 줄 수 있다.
- 이 때 사용하는 어노테이션이 @FunctionalInterface이다.

```java
package calculator;

@FunctionalInterface
public interface MyCalculator {
	int plus(int num1,int num2);
}
```

![image](https://user-images.githubusercontent.com/54658614/224226019-2b79461e-208f-4815-8563-ace38207c754.png)

### Calculator 클래스 만들기
```java
package calculator;

public class Calculator2 {
	public static void main(String[] args) {

		//인터페이스 객체를 익명 클래스 선언으로 정의
		MyCalculator cal = new MyCalculator() {

			//메서드 구현
			@Override
			public int plus(int num1, int num2) {
				return num1 + num2;
			}
		};
	}
}
```
### CalculatorImpl 클래스 생성하기
```java
package calculator;

public class CalculatorImpl {
	public static void main(String[] args) {
		//인터페이스 객체 선언 시 람다식을 이용해 함수를 구현
		MyCalculator calc = (int num1, int num2)->{
			return num1 + num2;
		};
		
		int result = calc.plus(20, 30);
		System.out.println(result);
	}
}
```
## 람다식과 외부변수의 관계
- 람다식을 사용할 때 매개변수로 값을 전달하는 것 외에 외부에서 정의 된 지역변수를 사용하는 경우가 있다.
- 람다식 내부에서 지역변수를 사용하려면 그 지역변수는 final로 선언되어야 한다.

### Calculator2 클래스 생성
```java
package calculator;

public class Calculator2 {
	public static void main(String[] args) {
		int num3 = 30;
		MyCalculator cal = new MyCalculator() {
			
			@Override
			public int plus(int num1, int num2) {//내부 익명클래스로 만들었더라도 인터페이스 이기 때문에
				//num3 = 40; -> final int num3 -> 지역변수의 상수화
				// TODO Auto-generated method stub
				return 0;
			}
		};
	}
}
```
- 지역변수는 Stack메모리 영역에 생성되고, 람다식의 경우 익명 객체를 만들기 때문에 Heap영역에 생성된다.
- 서로 생성되는 위치가 다르므로 간섭할 수 없다.
- 람다식 내부에서 지역변수를 사용할 경우 복사해 사용하므로 값을 그대로 사용하는 것은 가능하지만 수정할 수 없다.
- JDK1.8이전에는 람다식 또는 익명 클래스 안에 지역변수를 사용할 경우, final키워드를 부여해 변경 불가 변수임을 명시해야 했다.
- 그러나 JDK1.8이후 부터는 지역변수를 내부에서 사용할 때, 변경하지 않는다면 final변수로 인정해주는 effective final기능을 지원한다.

#### Calculrator3 클래스만들기
```java
package calculator;

public class Calculator3 {
	public static void main(String[] args) {
		MyCalculator calc = (int num1,int num2) ->{
			return num1 + num2;
		};
		
		//자료형도 생략이 가능하다 인터페이스에 정의가 되어있기 때문에!
		//자료형을 하나만 남겨놔도 안된다. 지우려면 다 지우자.
		//중괄호와 return도 생략가능하다.
		MyCalculator calc2 = (num1, num2) -> num1 + num2;
	}
}
```


### MyFunction 람다식 인터페이스 생성하기
```java
package calculator;

@FunctionalInterface
public interface MyFunction {
	//파라미터가 한개인 경우 생략해보기
	void method(int num);
}
```
### Calculator3 에 코드 추가하기
```java
package calculator;

public class Calculator3 {
	public static void main(String[] args) {
		MyCalculator calc = (num1, num2) ->{
			return num1 + num2;
		};
		
		//중괄호와 return도 생략가능하다.
		MyCalculator calc2 = (num1, num2) -> num1 + num2;
		
		//매개변수가 하나일 때는 소괄호가 생략이 가능하다.
		MyFunction myfunction = num -> System.out.println(num);
		
		//::(이중콜론) : 메서드 참조 연산자.
		//람다식을 보다 간결하게 사용할 수 있도록 해준다.
		MyFunction myfunc = System.out::println;
	}		
}
```

### Calculrator4 클래스 만들기
- 람다식을 메서드의 매개변수로 사용하기
```java
package test;

public class Test {
	public static void main(String[] args) {
		MyCalculator calc = (num1,num2) -> num1 + num2;
		int result = myCalc(calc);
		System.out.println(result);
		
		//위 방법보다 간단하게 표현할 수 있다.
		int result2 = myCalc((num1,num2) -> num1 + num2);
		System.out.println(result2);
	}
	
	//매개변수로 MyCalculator 객체를 필요로 한다.
	static int myCalc(MyCalculator calc) {
		
		//인터페이스에 정의된 plus메서드를 calc객체를 통해 호출한다.
		//이때 사용된 calc객체는 람다식으로 생성한 객체 또는 직접 메서드 호출시에 생성한
		//객체일 수 있으며, 이를 통해 다양한 계산을 수행할 수 있다.
		return calc.plus(1,2);
	}
}

```
### Calculrator5 클래스 만들기
- 컬렉션 프레임워크와 함수형 인터페이스
- 컬렉션 프레임워크의 인터페이스에 다수의 디폴트 메서드가 추가 되었고 그 중 일부는 함수형 인터페이스를 사용한다.
- ArrayList에 forEach()메서드가 있고 Consumer 라는 매개변수를 받는다 자바 공식문서에서 검색해보면 FunctionalInterface라는걸 알 수 있다.
- 때문에 람다식으로 표현할 수 있다.

-forEach()메서드 사용

```
void forEach(Consumer<? super T> action)
	↓↓↓↓
@FunctionalInterface
public interface Consumer {
    void accept(T t);
}
```

https://docs.oracle.com/javase/8/docs/api/java/util/function/Consumer.html

### Calculrator5 클래스 생성하기
```java
package calculator;

import java.util.*;

public class Calculrator5 {
	public static void main(String[] args) {
		ArrayList<String> list = new ArrayList<String>();
		list.add("이름1");
		list.add("이름2");
		list.add("이름3");
		list.add("이름4");
		list.add("이름5");
		
		list.forEach(x -> System.out.println(x));
		list.forEach(System.out::println);
	}
}
```
### Calculrator6 클래스 생성하기
- 람다식을 반환값으로 사용하기

```java
package test;

public class Calculator6 {
	public static void main(String[] args) {
		MyCalculator calc = myCalc();
		System.out.println(calc.plus(30, 50));
		
	}
	
	static MyCalculator myCalc() {
		//MyCalculator calc = (num1,num2) -> num1 + num2;
		//return calc;
		
		return (num1,num2) -> num1 + num2;
	}
}

```
## java.util.function패키지
- 대부분의 메서드는 타입이 비슷하다
- 매개변수가 없거나 한 개, 두 개, 반환값이 없거나 한 개이다.
- 제네릭 메서드로 정의하면 매개변수나 반환 타입이 달라도 문제가 되지 않는다. 
- java.util.function 패키지에 일반적으로 자주 쓰는 형식의 메서드를 함수형 인터페이스로 미리 정의해 놓았다.
- 매번 함수형 인터페이스를 정의하기 보다는 가능하면 이 패키지의 인터페이스를 활용하는 것이 좋다.

### java.util.function  패키지의 주효 함수형 인터페이스

|함수형 인터페이스|메서드|설명|
|----|-----|-----|
|java.lang.Runnable|void() run()|매개변수도 없고 반환값도 없음|
|Supplier<T>|T get()|매개변수는 없고 반환값만 있음|
|Consumer<T>|void accept(T t)|Supplier와 반대로 매개변수만 있고, 반환값이 없음|
|Function<T,R>|R apply(T t)|일반적인 함수. 하나의 매개변수를 받아서 결과를 반환|
|Predicate<T>|boolean test(T t)|조건식을 표현하는데 사용됨.   매개변수는 하나. 반환값은 boolean|

#### 참고) 타입문자 'T'는 'Type'을 'R'은 'Return Type'을 의미한다.

## 매개변수가 두 개인 함수형 인터페이스

매개변수가 두 개인 함수형 인터페이스는 이름 앞에 접두사 'Bi'가 붙는다.

|함수형 인터페이스|메서드|설명|
|----|-----|-----|
|BiConsumer<T,U>| void accept(T t, U u)|두개의 매개변수만 있고 반환값이 없음|
|BiPredicate<T,U>|boolean test(T t, U u)|조건식을 표현하는데 사용됨.   매개변수는 둘, 반환값은 boolean|
|BiFunction<T,U,R>|R apply(T t, U u)|두 개의 매개변수를 받아서 하나의 결과를 반환|

- 참고) Supplier는 매개변수는 없고 반환값만 존재하는데, 매서드는 두 개의 값을 반환할 수 없으므로 BiSupplier가 없다.
- 두 개 이상의 매개변수를 갖는 함수형 인터페이스가 필요하면 직접 만들어 써야 한다.

### Ex1_function 클래스 생성

![image](https://user-images.githubusercontent.com/54658614/224505449-4aeb4039-3830-4aea-a6be-3704f4759f39.png)

```java
package test;

import java.util.ArrayList;

public class Test {
	public static void main(String[] args) {

		ArrayList<Integer> list = new ArrayList<Integer>();
		for(int i = 1; i <=10; i++) {
			list.add(i);
		}
		list.removeIf(x -> x %2 == 0);//홀수만 반환
		System.out.println(list);
	}
}
```

## 람다식의 합성과 결합
- Function의 합성과 Predicate의 결합

### Function의 합성
```
default <V> Function<T, V> andThen(Function<? super R, ? extends V> after)
default <V> Function<V, R> compose(Function<? super V, ? extends T> before)
static <T> Function<T, T> identity() 
```
- 두 람다식을 합성해서 새로운 람다식을 만들 수 있다.
- f.andThen(g) - 함수 f를 먼저 적용하고 그 다음에 함수 g를 적용한다.
- f.compose(g) - g를 먼저 적용하고 f를 적용한다.
- Function.identity() - 함수를 적용하기 이전과 동일한 항등함수, x -> x

### f.andThen(g)
```
Function<String, Integer> f = (s) -> Integer.parseInt(s, 16);
Function<Integer, String> g = (i) -> Integer.toBinaryString(1);
Function<String, String> h = f.andThen(g);
System.out.println(h.apply("FF")); "FF" -> 255 -> "11111111"
```
### f.compose(g)
```
Function<Integer, String> g = (i) -> Integer.toBinaryString(i);
Function<String, Integer> f = (s) -> Integer.parseInt(s, 16); 
Function<Integer, Integer> h = f.compose(g);
System.out.println(h.apply(2)); // 2 -> "10" -> 16
```
### Function.identity()
```
Function<String, String> f = x -> x;
Function<String, String> f = Function.identity(); // 위 문장과 동일
System.out.println(f.apply("Hello")); // Hello가 그대로 출력됨
```
#### Ex4_function 클래스 생성
```
package day18;
import java.util.function.*;
public class FunctionComposeExam {
	public static void main(String[] args) {
						//Integer.parseInt(값, 진수);
		Function<String, Integer> f = (s) -> Integer.parseInt(s, 16);
						//이진수로 값 바꿔줌
		Function<Integer, String> g = (i) -> Integer.toBinaryString(i);
		
		Function<String, String> h = f.andThen(g);
		Function<Integer, Integer> h2 = f.compose(g);
		
		System.out.println(h.apply("FF")); // "FF" -> 255 -> "11111111"
		System.out.println(h2.apply(2)); // 2 -> "10" -> 16
		
		Function<String, String> f2 = x -> x; // 항등 함수(identity function)
		System.out.println(f2.apply("Hello")); // Hello가 그대로 출력됨
	}
}
```

### Predicate의 결합
```java
default Predicate<T> and(Predicate<? super T> other)
default Predicate<T> or(Predicate<? super T> other)
default Predicate<T> negate()   // 조건식 전체가 부정이 된다.
static <T> Predicate<T> isEqual(Object targetRef)
```
- Predicate를 and(), or(), negate()로 연결해서 하나의 새로운 Predicate로 결합할 수 있다.
- Predicate의 끝에 negate()를 붙이면 조건식 전체가 부정이 된다.
- static 메서드인 isEqual()은 두 대상을 비교하는 Predicate를 만들때 사용한다.

```java
package day18;
import java.util.function.*;
public class PredicateExam {
	public static void main(String[] args) {
		Predicate<Integer> p = i -> i < 100;
		Predicate<Integer> q = i -> i < 200;
		Predicate<Integer> r = i -> i % 2 == 0;
		Predicate<Integer> notP = p.negate(); // i >= 100
		
		Predicate<Integer> all = notP.and(q.or(r));
		System.out.println(all.test(150)); // true
		
		String str1 = "abc";
		String str2 = "abc";
		
		// str1과 str2가 같은지 비교한 결과를 반환
		Predicate<String> p2 = Predicate.isEqual(str1);
		boolean result = p2.test(str2);
		System.out.println(result);
	}
}
```

## 메서드 참조
- 메서드 참조는 말 그대로 메서드를 참조해서 매개변수의 정보 및 리턴 타입을 알아내어, 람다식에서 불필요한 매개변수를 제거하는 것
- 람다식이 하나의 메서드만 호출하는 경우에는 메서드 참조(method reference)라는 방법으로 람다식을 간결하게 할 수 있다.
- 하나의 메서드만 호출하는 람다식은 **클래스이름::메서드이름** 또는 **참조변수::메서드이름**으로 바꿀 수 있다.

※참조 타입 : (byte,short,char,int,long,float,double,boolean)기본 타입을 제외하고 배열, 열거, 클래스,인터페이스 등을 말한다.<br>
참조 타입의 변수에는 객체(메모리)의 주소가 저장된다.
 
```java
Function<String, Integer> f = (String s) -> Integer.parseInt(s);
Function<String, Integer> f = Integer::parseInt; // 메서드 참조
```

```java
BiFunction<String, String, Boolean> f = (s1, s2) -> s1.equals(s2);
BiFunction<String, String, Boolean> f = String::equals; // 메서드 참조
```

## 1. 정적 메서드 참조
- 클래스명::정적메서드명
```java
(x,y)-> Math.max(x,y);
```
- 함수 형태를 보면 리턴값 자체가 또다른 Math 클래스의 메서드를 호출하는 것 뿐이고,
- 람다 함수 매개변수 역시 그대로 max() 메서드의 매개변수로 들어가기 때문에 코드 중복이 발생한다.
```java
Math::max;
```
- 만약 Math:max 메서드 참조를 변수에 담고 싶다면 함수평 인터페이스인 IntBinaryOperator를 사용해야 한다.
- 두 개의 int형 입력값을 받아 int값을 리턴하는 함수 applyAsInt(int,int)를 가지고 있기 때문이다.
```java
package test;

import java.util.function.IntBinaryOperator;

public class Test {
	public static void main(String[] args) {
		IntBinaryOperator operator;
		
		operator = Math::max;
		
		operator.applyAsInt(100, 200);
	}
}
```

## 2. 인스턴스 메서드 참조
- 객체명::메서드명

### IAdd인터페이스 생성
```java
package test;

@FunctionalInterface
public interface IAdd {
	int add(int x, int y);
}
```

### MathUtils클래스 생성
```java
package test;

public class MathUtils {
	public static int AddElement(int x, int y) {
		return x + y;
	}
}
```

### MathMain클래스 생성
```java
package test;

public class Test {
	public static void main(String[] args) {
		MathUtils mu = new MathUtils();
		
		IAdd addLamda = (x,y) -> MathUtils.AddElement(x, y);
		System.out.println(addLamda.add(10, 20));
		
		IAdd addMethodRef = MathUtils::AddElement;
		System.out.println(addLamda.add(20, 40));
		
		
	}
}
```
