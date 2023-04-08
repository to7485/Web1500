## 제네릭스
### 제네릭스란?
- 일반적인 코드를 작성하고 이 코드를 다양한 타입의 객체에 대하여 재사용하는 객체 지향 기법이다.
- 객체의 타입을 컴파일 할 때 체크하기 때문에 객체의 타입에 대해 안정성을 높히고 형변환을 하는 번거로움을 줄일 수 있다.

ex1_generic 패키지 생성
#### GenEx 클래스 정의
우선 GenEx클래스를 만든후 <T>를 추가해야 한다. 클래스 이름을 정하는 과정에서는 <T>가 안들어간다.<br>
```java
public class GenEx<T>{
  //public class 클래스명<T>{}
  //public interface 인터페이스명<T>{}
  //T를 타입변수(type variable)이라 하며 Type의 첫글자에서 따온것이다.
  //다른 글자를 사용해도 되는데 E(Element),K(key),V(Value)를 많이 사용한다.
	T value;

	public T getValue() {
		return value;
	}

	public void setValue(T value) {
		this.value = value;
	}
}
```
#### GenEx_Main클래스 정의
```java
public class GenEx_Main {
	public static void main(String[] args) {
		// 사용자가 원하는 형태로 객체 생성
		GenEx<String> v1 = new GenEx<String>(); //T자리에 실제 타입을 지정한다.
		v1.setValue("100");

		System.out.println(v1.getValue());

		// 정수를 가지는 GenEx객체를 생성하자!
		// 제네릭 타입은 기본자료형을 인식하지 않음
		// 따라서 int, double등의 기본자료형을 제네릭타입으로 이용하고자 할 때는
		// Integer, Double등의 클래스를 이용해야 한다. 
  
		GenEx<Integer> v2 = new GenEx<Integer>();
		v2.setValue(1000);
		System.out.println(v2.getValue()+10);

		GenEx<Character> v3 = new GenEx<Character>();
		v3.setValue(‘A’);
		System.out.println(v3.getValue());

		GenEx<Double> v4 = new GenEx<Double>();
		v4.setValue(3.14);
		System.out.println(v4.getValue());
	}
}
```
### 멀티타입 파라미터
타입은 두개 이상의 멀티 타입 파라미터를 사용할 수 있고 이 경우 각 타입 파라미터를 콤마로 구분합니다<br>
```java
class People<T,M>{
    private T name;
    private M age;
	
    People(T name, M age){
        this.name = name;
        this.age = age;
    }

    public T getName() {
        return name;
    }
    public void setName(T name) {
        this.name = name;
    }
    public M getAge() {
        return age;
    }
    public void setAge(M age) {
        this.age = age;
    }
  }
```
#### ExGeneric 클래스 생성
```java
public class ExGeneric {
    public static void main(String []args){
        //타입 파라미터 지정
        People<String,Integer> p1 = new People<String,Integer>("Jack",20);
        //타입 파라미터 추정
        People<String,Integer> p2 = new People("Steve",30);
  
        System.out.println(p1.getName());
        System.out.println(p1.getAge());
        System.out.println("------------------");
        System.out.println(p2.getName());
        System.out.println(p2.getAge());
        System.out.println("------------------");
    }
} 
```
### 제네릭 메서드
- 메서드의 선언부에 지네릭 타입이 선언된 메서드를 제네릭 메서드라고 한다.
- 선언 위치는 반환 타입 바로 앞이다.

```java
public <T> void 메서드명(클래스명<T> 객체명)
```

 위 사람의 정보를 저장하는 코드에서 객체들의 이름이 일치하면 true, 다르면 false<br>
 나이가 일치하면 true, 다르면 false를 받아 논리연산하는 함수 만들기<br>
 ```java
   public static<T,V> boolean compare(People<T,V>p1, People<T,V>p2) {
      boolean nameCompare = p1.getName().equals(p2.getName());
      boolean ageCompare =p1.getAge().equals(p2.getAge());
      return nameCompare && ageCompare;
  } 
 ```

#### 문제로 내도 되고 같이 풀어도됨
Gen클래스를 만들어 제네릭 타입T를 갖는 printArr메서드를 생성한다.<br>
printArr메서드 내부에서 배열을 순차적으로 보여줄수 있게 하는 코드를 작성.<br>

Main클래스를 만들어 Integer[], Double[], Character[]을 각각 만든 뒤<br>
Gen클래스의 printArr메서드를 각각 호출하여 배열의 내용을 출력하도록 해보자.<br>

결과 :<br>
 1 2 3 4 5  //정수배열 출력<br>
 1.1 2.2 3.3 4.4 5.5 //실수배열 출력<br>
 A B C D E //문자배열 출력<br>
  
#### Gen클래스 정의
아래의 초록색 <T>는 둘 중에 한 개만 넣어주면 되지만,<br>
메서드에서 제네릭 타입을 사용할 경우 메서드쪽에 넣어주는 것이 더 좋다.<br>
```java
public class Gen <T> {
	public <T> void printArr(T[] arr){

		for(int i = 0; i < arr.length; i++){
			System.out.print(" " + arr[i]);
		}
		System.out.println();
	}
}
```  
#### GenEx클래스 정의
```java
public class GenEx{
	public static void main(String[] args) {
		Gen gen = new Gen();
		
		// 정수형
		Integer[] iArr = {1, 2, 3, 4, 5};

		// 더블형
		Double[] dArr = {1.1, 2.2, 3.3, 4.4, 5.5};

		// Character
		Character[] cArr = {'A', 'B', 'C', 'D', 'E'};

		//제네릭 이용
		//위의 배열들을 int, double, char와 같은 기본자료형으로 만들었다면
		//아래의 메서드에 대입할수 없다.
		//제네릭 타입은 반드시 객체를 처리하록 되어있다.
		gen.printArr(iArr);
		gen.printArr(dArr);
		gen.printArr(cArr);
	}
}
``` 
  
  
  
