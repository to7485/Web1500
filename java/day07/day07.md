## 사용자 정의 클래스
String 클래스는 문자열, Random 클래스는 난수, Scanner 클래스는 입력에 관한 각각의 기능이 있다면,<br>
이제는 우리의 입맛에 맞게 정의된 클래스를 한번 만들어보도록 하자<br>

S전자에서 컴퓨터를 생산한다고 가정하자.<br>
예를 들어 c-1이라는 모델의 컴퓨터가 양산되어야 한다면, 공장에서 컴퓨터를 대량으로 찍어내기 위해 필요한 컴퓨터의 부품이나, 기능 등을 명시한 설계도가 있을 것.<br>
이 설계도가 바로 클래스다.<br>

그리고 이 설계도를 바탕으로 만들어진 컴퓨터가 바로 객체.<br>

거의 비슷한 개념이기에 혼동하기 쉽지만 조금은 다른 내용이라는 것 정도만^^<br>

클래스는 한번 사용되고 버리는 것이 아니라, 재사용에 용이하게 작성함으로써 프로그램 상에서 낭비되는 자원을 줄일 수 있도록 구성하는 것이 좋다. <br>

이것은 자바의 가장 큰 특징인 ‘객체지향개념’과 결부되는데, 객체가 수행하는 능력이 뛰어나면 다른 곳에서도 그 객체를 사용하려는 횟수가 많아지게 되고 이런 재사용성을 목적으로 <br>
클래스에는 ‘속성(변수,  자료)’과 '기능(메서드, 함수)'를 정의하여 사용하게 된다.<br>

설계도에는 main이 포함되어 있지 않다. main같은 경우에는 설계도를 가지고 실제로 제품을 만들 때 사용한다.
```java
//1. 컴퓨터클래스 작성
public class Computer {
	//컴퓨터의 대량생산을 위한 설계도를 만드는 작업
	//클래스의 구성요소
	// 1) 멤버, 속성, 변수 ....
	// 2) 메서드, 기능, 함수 ...

	int sdd = 1024;
	int ram = 64;
	float cpu = 4.8f;
	String color = white
}

설계도를 가지고 물건을 만들어서 사양을 화면에 출력하여 살펴보자.
//1. 컴퓨터클래스Main 작성
public class ComMain {
	public static void main(String[] args) {
		
		Computer cp1 = new Computer();//객체 생성
		//아래쪽에 그림으로 설명 먼저!!
		System.out.println(“ssd : ” + c1.ssd);
		System.out.println(“ram : ” + c1.ram);
		System.out.println(“cpu : ” + c1.cpu);
		System.out.println(“color : ” + c1.color);

		
		System.out.println("-------------------------");
		
		Computer c2 = new Computer();
		System.out.println(“ssd : ” + c2.ssd);
		System.out.println(“ram : ” + c2.ram);
		System.out.println(“cpu : ” + c2.cpu);
		System.out.println(“color : ” + c2.color);
	}
}
```
불변의 법칙은 String 클래스에서만 적용이 된다. (new로 명시적 객체 생성을 해야하는 이유!)<br>
내 컴퓨터에 용량을 바꿔도 친구의 용량은 바뀌면안된다<br>

친구랑 나랑 똑같이 생긴 팬을 사용하는데 내가 다썼다고 해서 친구 팬의 잉크가 닳을 수 없다.<br>

```java
  //컴퓨터의 정보를 반환할 메서드를 만들자
	//메서드란 어떤 작업을 수행하기 위한 명령문들의 집합
	//반복적으로 사용되는 코드를 줄이기 위해서 반드시 필요한 개념
  
  //메서드란! 어떤 작업을 수행하기 위한 명령문의 집합이다!!!
  //메서드를 사용하는 작성하는 가장 큰 이유는 반복적으로 사용되는 코드를 줄이기 위해서이다.
  //자주 사용되는 내용의 코드를 메서드로 작성해 두고 필요할때마다 호출만 하면 된다.

	public void getInfo(){//메서드는 잠시후에 배운다. 일단 따라써
		System.out.println("하드 디스크 : " + hdd + "GB");
		System.out.println("램 : " + ram + "MB");
		System.out.println("cpu : " + cpu + "GHz");
		System.out.println("색상 : " + color);
	}
```
```java
//잠깐!! 메서드의 구성
	접근제한 반환형 메서드명  
	public void getInfo( 파라미터(인자)) {   메서드의 영역   }
```
### 접근제한자
접근제한자 에는 public, protected, default, private의 네종류가 있다.<br>
1. public : 모든 접근을 허용. 같은 프로젝트 내의 모든 객체들이 사용할 수 있도록 허용.<br>
2. private : 현재 클래스 내에서만 사용을 허가.<br>
3. protected : 상속관계의 객체들에만 사용을 허가.<br>
4. default : 같은 패키지(폴더)내의 객체에만 사용을 허가(아무것도 쓰지 않으면 default)<br>

1,2번이 자주 사용이 된다.

### 반환형
반환형은 메서드가 처음부터 끝까지 수행을 마친 후에 반환해야 할 값이 있을 경우에 기입.
int, String, boolean등 기본자료형을 포함하여 사용자가 만든 객체로도 반환이 가능.
아무것도 반환하지 않을때는 void

### 메서드명(함수명)
메서드명은 말그대로 메서드의 이름(첫글자는 소문자로 시작한다.)

### 파라미터(매개변수,인자,아규먼츠)
파라미터는 외부에서 해당 메서드를 통해 특정 값을 전달하고자 할 때, 그 특정 값을 받아서 처리할 수 있도록 하는 역할.

ex2_method 패키지 생성<br>
#### ValueTest 클래스 생성
```java
public class ValueTest {
	
	public void test(int n){
		n++;
		System.out.println("n : " + n);
	}// 메서드의 기능이 모두 끝났으면 호출한 곳으로 돌아간다.
	// 이때 반환형이 있으면 반환값을 가지고 돌아가지만
	// 반환형이 없는 void라면 빈손으로 돌아간다. 그리고 
	// 지역변수 n은 소멸된다.
}
```
#### ValueTestMain 클래스 생성
```java
public class ValueTestMain {
	public static void main(String[] args) {
		// 변수 선언과 값대입
		int su = 100;

		// test라는 메서드를 호출하기 위해 test메서드를 가지고 있는
		// 클래스를 생성한다.
		ValueTest vt = new ValueTest();//명시적 객체 생성
		vt.test(su);// su에 있는 값이 복사되어 전달된다.

		System.out.println("su: " + su);
	}
}
```
### 지역변수
- 특정구역{ } 내에서 생성되어 그 구역에서만 생성되고 소멸한다.
### 전역변수
- 어느 위치에서든지 호출하면 사용이 가능하다. 프로그램이 시작할때 생성되고 프로그램이 끝날때 소멸한다.

ex3_computer패키지 생성
### setter & getter
```java
//setter & getter 영원이 변할 것 같지 않지만 피치못할 사정으로 바뀔 때
//목적은 private으로 만들어진 변수의 값을 변경하거나 가져오고 싶을 때 사용하는 메서드 개념

coumputer클래스 생성
public class Computer {

      private String brand = “lucky gumsung”;
      int ssd = 512;

      public void setBrand(String a){//private에  있는 값을 세팅한다고 해서 setter
	brand = a;
      }
      
      public String getBrand() [//private에 있는 값을 가져 온다고 해서 getter
         return brand;
      }


}

ComMain클래스 생성
public class ComputerMain {
       public static void main(String[] args) {

      Computer com = new Computer();

      com.ssd = 1024; //클래스 안에 있는 변수로 접근하는 방법이 너무 간단함

      //com.brand = “apple”; 바꾸면 안되는 애들도 바뀌는 상황에서는 단점

      com.setBrand(“LG”);

      //System.out.println(com.brand); //값을 바꿀수 없지만 호출도 안됨
      System.out.println(com.getBrand());
      System.out.println(com.ssd);

          
    }//main
     

}

```
대량으로 생산되던 컴퓨터의 경우 설계도를 바탕으로 모두 같은 사양으로 만들어져 판매가 된다<br>
하지만 사람에 대한 정보를 저장하려고 하는 경우 사람마다 이름,나이,전화번호와 같은 정보가 모두 다를 수 있다.<br>
ex4_person 패키지 생성

사람의 정보를 저장하기 위한 Person클래스 생성
```java
public class Person {
        private int age;       //사람마다 이름,나이,전화번호가 다 다르기 때문에 미리설정x
	private String name; 
	private String email;
	private String phone;

        public int getProperty() {
		return age;
	}
	//메서드 에서는 지역변수가 우선이라 구분을 할 수 있게 this를 써야한다.
        public setProperty(int age) {
		this.age = age;
        }

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}
	
	// 전화번호를 변경하는 동작(메서드)
	public void setPhone(String n){
		// 현재 메서드는 문자열을 하나 받아서
		// 멤버변수인 phone에 저장하는 동작이다.
		this.phone = n;
	}
	
	//멤버변수 phone의 값을 반환하는 동작
	public String getPhone(){
		return this.phone;
	}	
}
-------------------------------------------------------------------------
PersonMain클래스 생성
public class PersonMain {
	public static void main(String[] args) {
		// 원하는 클래스를 객체화 시킨다.
		Person p1 = new Person();// 명시적 객체 생성

		//객체 p1으로 부터 getPhone, getName을 호출하여 반환받는 값을
		// 바로 출력하는 문장
		System.out.println(p1.getPhone());// null
		System.out.println(p1.getName());// null

		//객체 p1을 통해 정보를 저장한다.
    p1.setProperty( 20 );
		p1.setPhone("010-123-4567");
		p1.setName("쥐똥이");

		System.out.println(p1.getPhone());
		System.out.println(p1.getName());
	}
}

```

### 사용자 정의 클래스 실습문제
```java
//클래스로 계산기 만들어보기 문제.
/*
* 첫번째 숫자 입력 : 5
* 두번째 숫자 입력 : 10
* 연산기호 입력 : +
* 결과 : 15
* 
* Scanner를 사용해 
* 숫자 두 개와 연산기호를 받은 뒤 계산해주는 클래스를 만들고 실행하기
* 
* 참고: String의 비교는 ==아닌 String변수.equals("비교값")으로 한다
*  if(str.equals("+"))
*  else if(str.equals("-"))
*  else if(str.equals("*"))
*  else if(str.equals("/"))
*/


//풀이
public class CalTest {
	
	public int getResult(int n1, int n2, String str){
		
		if(str.equals("+"))
			return n1 + n2;
		else if(str.equals("-"))
			return n1 - n2;
		else if(str.equals("*"))
			return n1 * n2;
		else if(str.equals("/"))
			return n1 / n2;
		else{
			System.out.println("연산기호가 올바르지 않습니다.");
			return -1;
		}
	}
}

//CalMain클래스 구현
public class CalMain {
	public static void main(String[] args) {
		int n1, n2;
		String str;
		CalTest cal = new CalTest();
		
		Scanner sc = new Scanner(System.in);
		System.out.print("첫번째 숫자 입력 : ");
		n1 = sc.nextInt();
		
		System.out.print("두번째 숫자 입력 : ");
		n2 = sc.nextInt();
		
		System.out.print("연산기호 입력 : ");
		Scanner sc2 = new Scanner(System.in);
		str = sc2.next();//next와 nextLine차이점 설명

		System.out.print(“결과 : ”);
		System.out.println(cal.getResult(n1, n2, str));
	}
}
-----------------------------------------------------------------------
구구단 출력하기

문제설명 :
TimesTable클래스를 만들고

showTable()메서드를 정의한다.
showTable()메서드에는 구구단을 출력하는 코드를 작성.

TimesTableMain클래스를 만들어 TimesTable객체를 생성하고 
이를 이용하여 아래와같은 결과를 출력하자.

Scanner를 통해 값을 받는 작업은 반드시 TimesTableMain클래스에서 하도록 한다.

출력할 단을 입력 : 5
5단
5 * 1 = 5
5 * 2 = 10
5 * 3 = 15
5 * 4 = 20
5 * 5 = 25
5 * 6 = 30
5 * 7 = 35
5 * 8 = 40
5 * 9 = 45

풀이 :
TimesTable클래스 생성
public class TimesTable {		
	
	public void showTable(int num){
		
		System.out.println(num + "단");
		
		for(int i = 1; i <= 9; i++){		
			System.out.println(
				num + " * " + i + " = " + (num * i));
		}
	}
}
TimesTableMain클래스 생성
public class TimesTableMain {
	public static void main(String[] args) {
		
		int num;
		
		TimesTable tt = new TimesTable();
		
		System.out.print("출력할 단을 입력 : ");
		Scanner scan = new Scanner(System.in);
		
		num = scan.nextInt();
		
		tt.showTable(num);
	}
}
------------------------------------------------------------------
자바문제2(업다운)

문제설명 : 
Start클래스를 생성하여 1 ~ 50사이의 난수를 발생시킨다.
메인클래스를 만들고 사용자가 키보드를 통해 정수를 입력받는다.
Start클래스에서 사용자가 입력한 숫자를 판단하여 
발생한 난수보다 크다면 DOWN!! 작다면 UP!!을 출력.
사용자가 입력한 숫자와 발생한 난수가 같을경우에 프로그램을 종료시키며
몇회만에 정답을 맞췄는지 판단해보자.
단, 정답을 맞춘 경우 프로그램의 종료는 Start클래스가 아닌 
메인클래스에서 이루어 지도록 한다.

실행한 결과

숫자입력 : 30
Down!!
숫자입력 : 15
Up!!
숫자입력 : 25
3회 만에 정답!!!!

```java
public class Start {

	Random rnd = new Random();
	
	int rnum = rnd.nextInt(50)+1;
	int count = 1;
	public String check(int number) {
		if(number == rnum) {
			return "정답!";
		} else if(number >rnum) {
			return "DOWN!";
		} else {
			return "UP!";
		}
	}
}

main
public class StartMain {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		
		
		Start s = new Start();
		
		while(true) {
			System.out.print("숫자입력 : ");
			int number = sc.nextInt();
			if(s.check(number).equals("정답!")) {
				System.out.println(s.count+"회 만에 정답");
				break;
			} else {
				System.out.println(s.check(number));
				s.count++;
			}
			
		}
	}
}
```
```java

---------------------------------------------------------------
클래스를 이용한 입출금 로직 구현

문제설명 :
UserInfo클래스를 만든 뒤, 금액을 저장할 money라는 변수를 만든다.

deposit(int money)메서드를 만들어 유저가 돈을 입금했을 경우를 처리.

withdraw(int money)메서드를 만들어 유저가 돈을 출금했을 경우를 처리.
단 이 메서드에는 출금하고자 하는 돈 보다 잔액이 적을경우 잔액이 부족하다는 메시지가 출력되도록 한다.

showMoney()메서드를 만들어 현재 잔액을 반환하도록 한다.

UserInfo클래스는 여기까지. 
Main클래스를 새로 만들어 UserInfo형 객체를 생성한 뒤 아래의 결과가 나오도록 해보자.

1.입    금 : 
2.출    금 : 
3.잔액확인 : 
4.종    료 : 
1
---입   금---
입금할 금액을 입력 : 1000
입금성공

----------------------

1.입    금 : 
2.출    금 : 
3.잔액확인 : 
4.종    료 : 
3
---잔액확인---
1000원

----------------------

1.입    금 : 
2.출    금 : 
3.잔액확인 : 
4.종    료 : 
2
---출   금---
출금할 금액을 입력 : 5000
잔액부족


풀이 :
UserInfo클래스 생성
public class UserInfo {

	private int money; //잔액

	public void deposit(int money){
		System.out.println("입금성공");
		this.money += money;
	}

	public void withdraw(int money){

		if(this.money - money < 0){
			System.out.println("잔액부족");

		}else{
			System.out.println("출금성공");
			this.money -= money;
		}
	}
	
	public int showMoney(){
		return money;
	}
}


Main클래스 생성
public class Main {
	public static void main(String[] args) {

		int select;
		int money;

		UserInfo ui = new UserInfo();

		outer : while(true){
			System.out.println("1.입    금 : ");
			System.out.println("2.출    금 : ");
			System.out.println("3.잔액확인 : ");
			System.out.println("4.종    료 : ");

			Scanner scan = new Scanner(System.in);
			select = scan.nextInt();

			switch (select) {
			case 1:
				System.out.println("---입   금---");
				System.out.print("입금할 금액을 입력 : ");
				money = scan.nextInt();	
				ui.deposit(money);	
				break;

			case 2:
				System.out.println("---출   금---");
				System.out.print("출금할 금액을 입력 : ");
				money = scan.nextInt();	
				ui.withdraw(money);
				break;
				
			case 3:
				System.out.println("---잔액확인---");
				System.out.println(ui.showMoney() + "원");
				break;
				
				default :
					System.out.println("종료");
					break outer;//while문 탈출
			}
			
			System.out.println("----------------------");
		}//outer : while문의 끝
	}
}

-----------------------------------------------------------------

자바 강의 1주차(3) 문제(배열을 이용한 그래프)

Graph라는 이름의 메인 클래스를 만들어 0 ~ 9사이의 난수를 100개 저장하는 배열을 만들고, 해당 배열이 가지고 있는 각 방의 난수를 판별하여 그래프화 해 보자.

단, 발생한 난수의 그래프화 작업은 PrintGraph클래스가 하도록 한다.

결과:
0507...... //난수 100개
0의 갯수 : ############ 12
1의 갯수 : ######### 9
2의 갯수 : ########### 11
3의 갯수 : ######## 8
4의 갯수 : ############## 14
5의 갯수 : ####### 7
6의 갯수 : ######### 9
7의 갯수 : ############# 13
8의 갯수 : ####### 7
9의 갯수 : ########## 10

풀이 :
PrintGraph클래스 생성
public class PrintGraph {
	
	public String print(char ch, int num){
		char[] val = new char[num];
		String str = "";
	for(int i = 0; i < val.length; i++){
	
str += val[i] = ch;
}
		return str;
	}
}

Graph클래스 생성
public class Graph {
	public static void main(String[] args) {
	
int[] num = new int[100];//난수를 담을 배열
	
int[] count = new int[10];//발생한 난수가 각각 몇 개인지 저장할 배열
	for(int i = 0; i < num.length; i++){
	
//0 ~ 9사이의 난수
System.out.print(num[i] = new Random().nextInt(10));
}
	System.out.println();
	for(int i = 0; i < num.length; i++){
	count[num[i]]++;
	PrintGraph pg = new PrintGraph();
		for(int i = 0; i < count.length; i++){
			System.out.println(i + "의 갯수 : " + pg.print('#', count[i]) + " " + count[i]);
        }	
    }
}
```




