## 배열(Array)
배열은 같은 자료형의 변수들로 이루어진 유한 집합이라고 정의할 수 있다<br>
데이터를 효율적으로 관리하기 위해서는 배열이 꼭필요하다.<br>

ex1_array 패키지 생성

### 배열의 선언
![image](https://user-images.githubusercontent.com/54658614/215389995-4398cf1e-78c8-4969-8478-1a3fb9a741a1.png)

### 배열의 생성
![image](https://user-images.githubusercontent.com/54658614/215390064-8eaa5306-29c6-4aa4-9acb-86f377d1f8d2.png)

### 선언과 생성을 동시에 하는것도 가능하다.
![image](https://user-images.githubusercontent.com/54658614/215390219-ebbe71be-fd12-46b5-8d31-a12f98307c21.png)


### 1차원배열
```java
//ArrayEx1클래스 생성
public class ArrayEx1 {
	public static void main(String[] args) {
		//배열(array) : 같은 자료형끼리 모아둔 하나의 묶음
		// 효율적인 관리를 위해서 반드시 필요하다.

		// 정수변수 선언 -> 여러개가 필요하면 변수를 많이 만들어야 한다.
		int su1 = 100;
		int su2 = 200;
		int su3 = 300;
		int su4 = 400;

		// 편하게 자원들을 관리하고 제어하기 위해서는
		// 다음과 같은 배열을 생성한다.
		// 1) 배열선언
		int[] ar;

		// 2) 배열생성
		//배열의 index수의 갯수는 처음 지정해둔 갯수에서 강제로 늘리거나 줄일 수 없다.
		스택과 힙에 대한 설명  기본자료형 변수와 데이터는 스택에 만들어진다.
		클래스 구조와 배열구조는 스택에 일단 만들어진다. new 라는 키워드를 통해서 힙 메모리 영역에 집을 지어주는 키워드이다.
		ar = new int[4];

		//생성 후에는 값을 넣어 초기화가 필요하다.
		//아무런 값도 넣지않으면 기본자료형은 각 자료형의 초기값,
		//스트링형은 null이 들어간다.
		//int[] ar = {100, 200, 300, 400};//초기화 방법1

		// 3) 초기화 방법2
		ar[0] = 100;
		ar[1] = 200;
		ar[2] = 300;
		ar[3] = 400;
		//배열의 출력		
		System.out.println(arr[0]);
		System.out.println(arr[1]);
		System.out.println(arr[2]);
		System.out.println(arr[3]);	
		
		//배열의 출력2
		//ar.length : 배열의 방의 개수
		for(int I = 0; I < ar.length; I++){
			System.out.println(ar[i]);
		}
	}
}
```
![image](https://user-images.githubusercontent.com/54658614/215388446-0a8c33c5-5e62-4798-b98b-d445511574a6.png)

```java
public class Ex2_array {
	public static void main(String[] args) {
		
		int [] iArr = new int[4];
		
		for(int i = 0; i < iArr.length; i++) {
			//초기화를 할 때 값을 반복문을 통하여 한번에 할당을 할 수 도 있다.
			iArr[i] = (i + 1)*100;
			System.out.println(iArr[i]);
		}
		
	}//main
}
```
### 문자형 배열
```java
//ArrayEx2클래스 생성
public class ArrayEx3 {
	public static void main(String[] args) {
		char[] ch;
		ch = new char[4];
		//char[] ch = new char[4];
		
		//배열 초기화
		ch[0] = 'J';
		ch[1] = 'A';
		ch[2] = 'V';
		ch[3] = 'A';
		
		//배열 내용 출력
		for(int i = 0; i < ch.length; i++){
			System.out.printf("ch[%d] : %c",i,ch[i]);
		}


		String[] str = new String[3];
		str[0] = "hello";
		
		for(int i = 0; i < ch.length; i++) {
			System.out.print(ch[i]);
		}
		System.out.println();
    
    System.out.println(ch); //문자형 배열은 배열의 이름만으로도 전체 내용을 출력할 수 있다.
		
		System.out.println("----------------------------");
		
		System.out.println(str); //문자열 배열은 이상하게 나온다.

		//개선된 루프.
		//편리하지만, 배열의 각 요소에 대한 값의 수정과 삭제가 불가
		//for(char ch2 : ch)
			//System.out.println(ch2);
	}
}
```

### 실습문제
```java
//배열 arr에 담겨있는 모든 값의 합을 출력하시오
//결과 : 150

int[] arr = {10,20,30,40,50};

int total = 0;
for(int I = 0; I <arr.length; I++){
	total += arr[i];
}
System.out.printf(“배열의 총 합 : %d\n”,total);
-------------------------------------------------------------------------------
프로그램이 실행되면 배열의 길이를 몇으로 할 것인지 물어본다.
예를들어 키보드에서 5를 입력받으면...

결과 : 
배열의 길이는 몇으로? : 5
ABCDE

public class Work_Ex1 {
	public static void main(String[] args) {
		
		System.out.print("배열의 길이는 몇으로? : ");
		Scanner scan = new Scanner(System.in);
		int n = scan.nextInt();
		
		char c[] = new char[n];
		char c2 = 'A';
		
		for(int i = 0; i < c.length; i++){
			System.out.print(c[i] = c2++);
		}
		
		// char ch[] = new char[n];
		// for(int i = 0; i < ch.length; i++){

		 // ch[i] = (char)('A' + i);
		 // System.out.print(ch[i]);
		// }		
	}
}
-------------------------------------------------------------------------------
문제. 
변수 money에 10 ~ 5000사이의 난수를 발생시켜 넣는다.
단 3450,2100,60과 같이 1의자리 숫자는 반드시 0이 되도록 한다.

발생된 난수 money를 동전으로 바꾸면 각 동전이 몇 개씩 필요한지를 판단하는 코드 작성,

가능한한 적은 수의 동전을 사용하도록 한다.

//4170
//500원 : 8
//100원 : 1
//50원 : 1
//10원 : 2

int[] coin = {500,100,50,10};
int money = new Random().nextInt( 500 )+1
money *= 10;
System.out.println(“금액: ” + money);

for(int I = 0; I < coin.length; I++) {
     int res = money / coin[i];
     System.out.println( coin[i] + “원 : ” + res);
     money %= coin[i];
}//for
-------------------------------------------------------------------------------
문제. 
1 ~ 45의 난수를 발생시켜 로또번호를 생성하는 프로그램 만들기.
public class MyLotto {
	public static void main(String[] args) {
		
		//로또번호 6개를 담을 배열 준비
		int[] lotto = new int[6];
		outer : for(int i = 0; i < lotto.length;){//나중을 위해 i++을 생략
			lotto[i] = new Random().nextInt(45) + 1;
			//중복값을 비교하는 반복문
			for(int j = 0; j < i; j++){
				if(lotto[i] == lotto[j]){
					continue outer;
				}					
			}//inner For
			System.out.print(lotto[i] + " ");
			i++;						
		}//outer For
	}
}
```

### 다차원 배열
다차원 배열이란 2차원 이상의 배열을 의미하며, 배열의 요소로 또 다른 배열을 가지는것을 의미합니다.<br>

2차원 배열은 배열의 요소로 1차원 배열을 가지고,
3차원 배열은 배열의 요소로 2차원 배열을 가지게 됩니다.

![image](https://user-images.githubusercontent.com/54658614/215390365-cc5e7984-8f1a-42e7-b748-0e9a4b816ead.png)

2차원 까지는 많이 사용 하지만 3차원부터는 거의 사용되지 않는다.

```java
int test[][] = new int[2][3];
test[0][0] = 100;
test[0][1] = 200;
test[0][2] = 300;
		
test[1][0] = 400;
test[1][1] = 500;
test[1][2] = 600;
System.out.println(test[0][1]);//숫자 바꿔가며 확인
```
![image](https://user-images.githubusercontent.com/54658614/215390509-ba91a4f5-ca52-41e0-bd32-04be2d2e08d8.png)


```java
--------------------------------------------------------------------예제1

String[][] java = {{"영희", "java : 100", "android : 90"}, 
		{"철수","java : 80", "android : 75", "jsp : 98"}};//이런식으로 정의 가능
		
		
for(int i = 0; i < java.length; i++){ 
			
	for(int j = 0; j < java[i].length; j++){
		System.out.println(java[i][j]);
	}
			
	System.out.println();
}

--------------------------------------------------------------------예제2

2차원 배열부터는 큰방에 있는 작은방들이 크기가 다를 수 있다.

char[][] ch = { { ‘A’, ‘B’},
               {‘C, ’D’,‘E’},
               {‘F’} };

//출력해보기

for(int I = 0; i< ch.length; I++) {

    for(int j = 0; j < ch[i].length; j++) {
            System.out.print(ch[i][j] + “ ”);
    }//inner
        System.out.println();
}//outer
```
```java
↓↓↓이렇게 각 방 사이즈 지정해줘도 됨
int num[][] = new int[2][];
num[0] = new int[3];
num[1] = new int[2];
int n = 0;
		
for(int i = 0; i < num.length; i++){
			
	for(int j = 0; j < num[i].length; j++){
				
		System.out.print((num[i][j] = n += 100) + " ");
				
	}
	System.out.println();		
}
```

### 실습문제
```java
학생들의 수학과 영어성적을 등록하는 프로그램이 있다.
프로그램을 실행하면 몇 명의 정보를 저장 할 것인지를 입력받은 후,
입력받은 수 만큼 학생들의 이름과 수학성적, 영어성적을 입력받는 프로그램 작성 

결과 :
등록할 인원수 : 2
이름 : 홍길동
수학 : 90
영어 : 87
-------------------------
이름 : 독고길동
수학 : 70
영어 : 100
-------------------------
2명 등록 완료!!
홍길동 90 87
독고길동 70 100


풀이 : 
public class Work_Ex2 {
	public static void main(String[] args) {

		System.out.print("등록할 인원수 : ");
		Scanner scan = new Scanner(System.in);
		int n = scan.nextInt();

		String str[][] = new String[n][3];

		for(int i = 0; i < str.length; i++){

			System.out.print("이름 : ");
			str[i][0] = scan.next();

			System.out.print("수학 : ");
			str[i][1] = "수학 : " + scan.next();

			System.out.print("영어 : ");
			str[i][2] = "영어 : " + scan.next();
			System.out.println("-------------------------");
		}		
		
		System.out.println(str.length + "명 등록 완료!!");
		for(int i = 0; i < str.length; i++){
			
			for(int j = 0; j < str[i].length; j++){
				System.out.print(str[i][j] + “ ”);
			}
			System.out.println();
		}
	}
}
----------------------------------------------------------------------------
2차원 배열을 만들고 아래와같이 공간을 채워넣는다.

int arr[][] = {{1, 2, 3, 4, 5},
		{6, 7, 8, 9, 10},
		{11, 12, 13, 14, 15},
		{16, 17, 18, 19, 20}};

2차원 배열 arr에 담긴 모든 값의 총 합과 평균을 구하는 프로그램을 완성해보자.

풀이 : 
public class FileEx {
	public static void main(String[] args) throws Exception{

		int arr[][] = {{1, 2, 3, 4, 5},
				{6, 7, 8, 9, 10},
				{11, 12, 13, 14, 15},
				{16, 17, 18, 19, 20}};
		
		//int arr[][] = new int[4][5];
		//int count = 0;
		int total = 0;
		float average = 0;
		int count = 0;

		for(int i = 0; i < arr.length; i++){

			for(int j = 0; j < arr[i].length; j++){
				//arr[i][j] = ++count;
				total += arr[i][j];
				count++;
			}
		}
		System.out.println("total : " + total);
		average = (float)total / count;
		System.out.println("평균 : " + average);
	
	}
}

```
## String 클래스
자바로 만들어진 모든 프로그램은 클래스로 이루어져 있습니다.
우리가 문자열을 저장하기 위해 사용했던 String도 자바에 내장되어있는 클래스입니다.

[자바문서](https://docs.oracle.com/javase/8/docs/api/index.html)

```java
package ex_String;

import java.util.Scanner;

public class Ex1_String {
		public static void main(String[] args) {
			
			//String클래스는 두가지 특징을 갖고 있다.
			//1)객체 생성법이 두가지(암시적, 명시적)
			//2)한 번 생성된 문자열의 내용은 변하지 않는다(불변의 특징)
			
			String s1 = "abc";//암시적 객체생성
			String s2 = "abc";
			//이미 앞에 같은 문자열로 생성된 암시적 객체가 있다면
			//앞서 생성된 객체의 주소를 재사용한다.

힙영역에 데이터가 생성되는건 모두 객체라고 한다.
			
			String s3 = new String("abc");//명시적 객체 생성
			String s4 = new String("abc");

자바의 모든 클래스들 중에서 new를 사용하지 않고 만들 수 있는건 String 밖에 없다. String도 클래스인데 마치 자료형처럼 사용할 수 있는 이유.
			
			
			// == 연산자는 기본자료형을 비교할 때는 값을 비교한다.
			//그러나 객체끼리 비교를 할 때는 주소를 비교하는 연산자로 기능이 바뀐다.
			if(s1 == s2) {
				System.out.println("주소가 같습니다.");
			} else {
				System.out.println("주소가 다릅니다.");
			}
			System.out.println("-------------------------------");
			if(s3 == s4) {
				System.out.println("주소가 같습니다.");
			} else {
				System.out.println("주소가 다릅니다.");
			}
			System.out.println("-------------------------------");
			
			if(s3.equals(s4)) {
				System.out.println("내용이 같습니다.");
			} else {
				System.out.println("내용이 다릅니다.");
			}
			System.out.println("------------------------------");
			
스캐너를 사용을 해봤는데 코드를 다시 한번 살펴 보자.

			Scanner sc =new Scanner(System.in);
			System.out.println("문자열: ");
			String s5 = sc.next();
			
			
			if(s5 == s1) {
				System.out.println("값이 같습니다.");
			} else {
				System.out.println("값이 다릅니다.");
			}
			
			System.out.println("--------------------------------");
			//불변의 특징
			
			
			String greet = "안녕";
			greet +="하세요";
			System.out.println(greet);
			
처음에 안녕 이라고 적인 메모리 영역이 힙에 할당 되는데 하세요가 붙는순간 안녕 뒤에 붙는게 아닌
안녕하세요 라고 하는 메모리를 새로 할당 받는다 그러면 남아있는 안녕이 메모리를 낭비시킬수 있는데
JVM의 GC가 힙 영역을 돌며 사용하지 않는 쓰레기 데이터를 주워간다.
			
			
			
		}//main
}
```
### String 클래스의 메서드들
메서드란! 어떤 작업을 수행하기 위한 명령문의 집합이다!!!<br>
메서드를 사용하는 작성하는 가장 큰 이유는 반복적으로 사용되는 코드를 줄이기 위해서이다.<br>
자주 사용되는 내용의 코드를 메서드로 작성해 두고 필요할때마다 호출만 하면 된다.<br>
```java
indexOf, lastIndexOf, charAt, substring, split정도만 설명. api보면서 하면 더 좋겠지만
융통성있게 하자


String str = "Kim Mal Ddong";
System.out.println("문자열 str의 길이 : " + str.length());
		
int index = str.indexOf('k');
System.out.println("맨 처음 문자 k의 위치 : " + index);//대소문자 구별 함
	
index = str.indexOf("Mal");
System.out.println("문자열 Mal의 위치 : " + index);//띄어쓰기 포함
	
index = str.lastIndexOf('n');
System.out.println("마지막 문자 n의 위치 : " + index);
		
char c = str.charAt(4);
System.out.println("추출한 문자 : " + c);
		
String str2 = str.substring(0, str.indexOf('M'));
System.out.println("0번째부터 M앞자리까지 글자 잘라내기 : " + str2);
System.out.println("잘라낸 str2의 길이 : " + str2.length());//길이는 띄어쓰기 포함 1부터 증가

스트링은 아니지만 스트링으로 작성된 숫자형태의 문자열을 실제 정수로 바꿔주는 코드
String number = "1";
System.out.println(Integer.parseInt(number) + 10);

int number = 1;
String s1 = Integer.toString(number);
System.out.println(s1);
		
String arr[] = str.split(" ");//띄어쓰기를 기준으로 분할

for(int i = 0; i < arr.length; i++)
	System.out.println("arr[" + i + "] : " + arr[i]);
```

### String 클래스의 메서드를 이용한 실습문제
```java
키보드에서 숫자와 특수문자를 제외한 알파벳을 무작위로 입력받는다.
입력받은 문자열에 소문자 a가 몇 개 있는지를 판별하는 로직을 구현해보자.

결과 : 
입력 : asdfasdf
a의 갯수 : 2

풀이 : 
public class Work_Ex1 {
	public static void main(String[] args) {

		String str;
		int count = 0;
		
		System.out.print("입력 : ");
		Scanner scan = new Scanner(System.in);
		str = scan.next();
		
		for(int i = 0; i < str.length(); i++){
			if(str.charAt(i) == 'a'){
				count++;
			}
		}
		
		System.out.println("a의 갯수 : " + count);
	}
}
-------------------------------------------------------------------------
자바 강의 1주차(3) 문제(String) - 3
회문수 구하기.
회문수란 앞으로 읽어도 뒤로 읽어도 똑같이 읽히는 숫자를 말합니다. 예를들어 12121과 같은 숫자.

키보드에서 세자리 이상의 숫자를 입력받은 후 해당 숫자가 회문수인지 아닌지를 판단하는 로직을 구현하자.

단, String클래스의 메서드를 활용하는 문제이니만큼, 키보드에서 입력받는 숫자는 String변수에 담아서 활용할수 있도록 한다.

풀이 :
public class Work_Ex3 {
	public static void main(String[] args) {
		
		String str = "";
		String str2 = "";
		
		System.out.print("3자리 이상의 숫자를 입력하세요 : ");
		Scanner scan = new Scanner(System.in);
		str = scan.next();
		
		for(int i = str.length(); i > 0; i--){
			str2 += str.charAt(i-1);
		}
		
		if(str.equals(str2)){
			System.out.println(str + "은 회문수 입니다.");
		}else{
			System.out.println(str + "은 회문수가 아닙니다.");
		}			
	}
}
-------------------------------------------------------------------------
자바 강의 1주차(3) 문제(String) - 4
아래와 같은 결과를 반영하는 로직을 구현해보자.

결과)

주민번호를 모두 입력하세요(-포함)
예)911223-203345
>> 991122-1122333
당신은 1999년 11월 22일에 태어난 남자입니다.


풀이)
public class Work {
	public static void main(String[] args) {
		
		System.out.println("주민번호를 모두 입력하세요(-포함)");
		System.out.println("예)911223-203345");
		System.out.print(">> ");
		Scanner scan = new Scanner(System.in);
		String id = scan.next();

		if(id.trim().length() < 14 || id.trim().charAt(6) != '-'){

			System.out.println("주민번호를 올바르게 입력하세요.");

		}else{

			String year = "";
			String result = "";

			year = id.substring(0, 2);

			if(Integer.parseInt(year) <= 14){
				result = "당신은 20";
			}else{
				result = "당신은 19";
			}
			result += year + "년 "
					+ id.substring(2, 4) +"월 " 
					+ id.substring(4, 6)
					+ "일에 태어난 ";

			if(id.charAt(7)%2 != 0){
				result += "남자입니다.";
			}else{
				result += "여자입니다.";
			}

			System.out.println(result);

		}//else

	}//main
}
```





