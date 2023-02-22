# 실습 환경 구축하기

## 내 PC에 실습을 위한 폴더 만들기
내 PC -> D드라이브 진입 -> JAVA1900_이니셜 -> 안에 util,work 폴더만들기

![image](https://user-images.githubusercontent.com/54658614/211975967-b510f6c6-1f98-4372-b52c-65c6ed39bb6e.png)

<hr>


## 자바 설치하기
- [JDK다운로드](https://www.oracle.com/java/technologies/downloads)

### 자바의 버전을 보면 많다 어떤 버전을 사용하는것이 좋은가??
기업에서 이미 운영하고 있는 프로젝트들이 있을 것이다. 대부분 Java8버전으로 하는 경우가 많다.
프로젝트를 새로 시작하는 경우 Java11, Java17을 사용하는 4경우가 있다.

### 특정 자바 버전을 학습 해야 할까???
12,17과 같은 특정 Java버전만을 "학습"할 필요가 없다.
자바는 하위호환성이 매우 높기 때문이다.
<hr>

## 통합 개발환경
통합 개발 환경(統合開發環境, Integrated Development Environment, IDE)은 코딩, 디버그, 컴파일, 배포 등 프로그램 개발에 관련된 모든 작업을 하나의 프로그램 안에서 처리하는 환경을 제공하는 소프트웨어이다.

### 대중적인 IDE
- 이클립스(Eclipse) : 웹, Java, C, C++
- Visual Studio : C, C++, C#, Python
- Intellij : Java, C/C++
등등 많은 IDE가 있다.

## 이클립스 설치하기
- [Eclipse다운로드](https://www.eclipse.org/)
<hr>

## 자바란?
썬 마이크로시스템즈 소속 제임스고슬링 등의 일부 연구진들은 '그린프로젝트'라는 이름으로 '오크(Oak)'라는 언어를 개발하고 있었다.<br>오크는 오디오,tv,세탁기 등 각각의 가전제품을 제어하는 통합된 언어로써 개발중이었지만 결국 목적을 달성하지 못하고 실패로 돌아간다.<br>그 무렵 웹(www)이 급속도로 발전하게 되고, 이에 발맞추고자 썬에서는 오크의 명칭을 Java로 바꾼뒤 서로 다른 컴퓨터(OS - 운영체제)사이에서 호환성과 이식률을 높인 언어로 발전시켰다.

![image](https://user-images.githubusercontent.com/54658614/211973993-8e9d86f8-5251-48b8-9e10-4567dde3ef49.png)

<hr>

## 자바가 잘 설치되어있는지 확인하는법

win + r -> 실행창에 cmd -> java -verion 입력

![image](https://user-images.githubusercontent.com/54658614/211975334-e2287139-5269-4218-8dd0-1167fd62eea4.png)

```diff
- ※ jdk를 먼저 설치하지 않으면 이클립스가 실행되지 않습니다!!
```

<hr>

## 이클립스 실행해보기
이클립스를 켜고 Browse를 눌러 우리가 만들어 놓은 work 폴더로 경로를 잡아주자.(앞으로 우리가 작업하는 프로젝트들은 work폴더에 저장될 것이다.)

![image](https://user-images.githubusercontent.com/54658614/211976280-0159d649-c5d5-47c3-840a-09f4e37d4e24.png)

<hr>

## 이클립스 프로그램 구성

![image](https://user-images.githubusercontent.com/54658614/212237771-1da23aee-2b7b-4d49-9692-e1b87980a835.png)

## 자바 프로젝트의 구성
자바는 패키지라고 하는 폴더 단위로 프로젝트를 관리한다.<br>
내가 손흥민이 최근에 넣었던 골에 대한 영상을 찾고 싶은데 하나의 폴더에 축구 동영상 50개와 야구 동영상 50개가 있다면 1/100 확률로 찾을 수 있다.
하지만 최소한 축구 동영상 폴더와 야구 동영상 폴더를 나눠서 관리를 하게 되면 1/50 확률로 찾을 수 있는 속도가 2배 증가한다.

## 자바 프로젝트 생성하기
![image](https://user-images.githubusercontent.com/54658614/212238723-e89acb35-1605-4a1b-8e9f-716fd097cc81.png)

![image](https://user-images.githubusercontent.com/54658614/212238789-2ef70972-6e66-432e-9380-159f46f01526.png)

![image](https://user-images.githubusercontent.com/54658614/212238858-e61b8d78-c9a4-45ee-9480-ad523f1589ad.png)

![image](https://user-images.githubusercontent.com/54658614/212238952-7869b3bd-a2ea-4eeb-a94a-5081bedc8ecc.png)

## 폴더에 해당하는 패키지 생성하기

![image](https://user-images.githubusercontent.com/54658614/212239071-7dea18ab-2129-470a-b383-ae8a81c6a9d6.png)

### 패키지 이름을 적고 finish를 누릅니다.

![image](https://user-images.githubusercontent.com/54658614/212239347-bc640c8d-266a-4e4a-b1cf-6ca13aad557b.png)

### 그러면 비어있는 패키지가 생성되게 됩니다.

![image](https://user-images.githubusercontent.com/54658614/212239455-3f18af92-83b4-4f2f-8c43-276e56506204.png)

## 코드를 작성하는 공간인 클래스 생성하기

![image](https://user-images.githubusercontent.com/54658614/212239571-0380fc1a-b15c-4f17-aacd-036e5e6d9b05.png)

### 클래스를 작성할 때 클래스의 이름의 첫번째 글자는 반드시 대문자로 작성해야 합니다.

![image](https://user-images.githubusercontent.com/54658614/212239611-d741c393-40b5-430b-8b43-3b8e62956724.png)

![image](https://user-images.githubusercontent.com/54658614/212239789-91e1818d-fb55-4fba-83b5-c727a4cbae4c.png)

### 클래스를 작성하면 항상 main이라고 하는걸 작성해주자 지금 당장은 뭔지 모르겠지만 자바가 실행될 때 main 영역 안에 있는 코드가 실행된다.

```java
public class Test {
	public static void main(String[] args) {
		
	}
}
```
### System.out.println("hello world"); 작성해보기
main의 영역 안에 System.out.println("hello world")를 적고 ctrl + f11를 눌러 실행을 해보도록 하겠습니다.<br>
실행을 하기 전에는 ctrl + s를 눌러 꼭 저장을 해주도록 합시다!
```java
public class Test {
	public static void main(String[] args) {
		System.out.println("hello world");
	}
}
```
저장을 하고 실행을 하면 콘솔이라는 공간에 System.out.println() 소괄호 사이에 적은 문장이 출력되어 나오는 모습을 볼 수 있습니다.
![image](https://user-images.githubusercontent.com/54658614/212241733-b89dbf5a-ae42-4ebd-b28c-53d89afbc2af.png)

## 주석
프로그램의 소스코드에 프로그래머의 의견이나 설명을 적을 수 있는데 이런 것을 주석(Comment)라고 합니다. 주석은 프로그램 소스에 삽입하더라도 프로그램의 수행에 전혀 영향을 끼치지 않습니다. 컴퓨터(JVM)에서 컴파일을 할 시 인식하지 못하는 코드이기 때문입니다.

```java
public class Test {
	public static void main(String[] args) {
	    //한줄 주석 : //
	    /*
	    여
	    러
	    줄
	    주
	    석
	    */
	    //주석을 사용하는 이유 : 코드에 설명을 달아주기 위해
	    //sysout 적고 ctrl + spacebar 자동완성
		System.out.println("hello world");
	}
}
```

## JVM
### JVM이란?
자바 가상 머신(Java Virtual Machine)은 자바 프로그램 실행환경을 만들어주는 소프트웨어입니다. 자바 코드를 컴파일 하여 바이트 코드로 만들면 이 코드가 자바 가상 머신 환경에서 실행됩니다. JVM은 자바 실행 환경 JRE(Java Runtime Environment)에 포함되어 있습니다. 현재 사용하는 컴퓨터의 운영체제에 맞는 자바 실행환경(JRE)가 설치되어 있다면 자바 가상 머신이 설치되어 있다는 뜻입니다.

## 컴파일(Compile)
프로그래머가 작성한 .java 코드(자바코드)를 .class 코드(바이트 코드)로 바꾸는 일련의 과정

## 여러가지 형태의 데이터 출력해보기

```java
public class Test {
	public static void main(String[] args) {
		    //한줄 주석 : //
		    /*
		    여
		    러
		    줄
		    주
		    석
		    */
		    //주석을 사용하는 이유 : 코드에 설명을 달아주기 위해
		    //sysout 적고 ctrl + spacebar 자동완성
		System.out.println("hello world");
		System.out.println(100);
		System.out.println(100+50);
		
		//문장뒤에 숫자를 더하면 문장 뒤에 붙는구나
		System.out.println("안녕하세요"+10);
		
		//코드는 항상 위에서 아래로, 좌에서 우로 진행이 되기 때문에 15:15가 나옵니다.
		System.out.println(5+10+":"+(5+10));
		
		// "2 + 2 = "가 숫자 처럼 보이지만 ""안에 묶여있으면 문장 취급을 받고
		//문장에 숫자를 더했기 때문에 22가 됩니다.
		System.out.println("2 + 2 = " + 2 + 2);
		System.out.println("2 + 2 = " + (2 + 2));	 
	}
}
```

## 자료형(기본자료형)
현실에서는 물을 종이컵에다 마시든, 플라스틱 컵에다 마시든, 유리컵에다 마시든 전혀 문제가 되지 않습니다.<br>
하지만 프로그래밍에서는 '물은 종이컵에 담아먹겠다' 라고 약속을 했으면 무조건 지켜야 합니다.<br>

프로그래밍에서 자료형이라고 하는거는 데이터를 담을 컵의 크기와 재질이라고 비유를 들 수 있습니다.

```
논리형 : boolean - 1bit (기본값 false)
문자형 : char - 2byte (기본값 \u0000 or 0) ____ 0 ~ 65,535
정수형 : byte - 1byte (기본값 0) ____ -128 ~ 127<br>
	 short - 2byte (기본값 0) ____ -32,768 ~ 32,767

	 int - 4byte (기본값 0) ____ -21,4748,3648 ~ 21,4748,3647(21억)
		 - 일반적으로 가장많이 쓰이는 자료형.<br>
		 - but 주식시장이나 증권 관련 분야에서는 long을 사용하는 경우가 더 많음

	  long - 8byte (기본값 0) ____ -9,223,372,036,854,775,808 ~ (900경)
실수형 :  float - 4byte (기본값 0.0)
	 double - 8byte(기본값 0.0)
```
## 변수
자료형이 데이터를 담기 위한 컵의 재질과 크기라면<br>
변수는 데이터를 실제로 담기 위한 컵을 만드는 과정 이라고 생각하시면 됩니다.<br>

### 변수의 선언

1. 변수를 선언하고 값을 대입하는 법 (컵을 만든 후 물을 따르는법)<br>
[자료형] 변수명; -> 변수의 선언<br>
변수명 = 값; -> 변수의 대입<br>

2. 변수를 만듦과 동시에 값을 넣는법<br>
[자료형] 변수명 = 값; -> 변수의 초기화(초기화 라고 함은 reset의 개념이 아닌 초기값을 지정한다는 의미)<br>

### 변수명 명명 규칙
1. 숫자가 먼저 들어가면 안된다.
2. _를 제외하고 특수기호가 포함될 수 없다.
3. 예약어 금지(switch, while 등)
4. 의미있는 단어로 이름짓자(숫자 num, 이름 name등)
5. 한글은 사용하지 말 것.

## 실습
```java
-----------------------------------------------------------------------------
논리형
논리형은 true, false 즉, 사실이다와 사실이 아니다의 두 가지 값만을 가진다.
boolean b = true;
System.out.println("b의 값 : " + b); //+기호는 더한다는 의미가 아닌 이어붙인다의 의미.
//단, 숫자 사이의 + 기호는 더하기를 의미.
boolean b = 1;//자료형의 값이 올바르지 않아 오류
-----------------------------------------------------------------------------

문자형
char ch = 'A'; //문자형은 홑따옴표 안에 넣어야 하며 한글자이상 넣을 수 없다.
System.out.println("ch = " + ch); //결과 : A
char ch2 = '\u0041'; //문자열이 들어간 것 같지만, 유니코드로 A를 의미하는 한글자가 들		 //어간 것.
System.out.println("ch2 = " + ch2); //결과 : A

char ch3 = 65 + 1; //아스키코드 65에 + 1
System.out.println("ch3 = " + ch3); //결과 : B
-----------------------------------------------------------------------------

정수형
byte b = 128; //byte자료형의 표현범위를 벗어나므로 오류가 난다.

byte b = 127;
short s = 32767;
int n = 550;

System.out.println("b = " + b); //결과 127
System.out.println("s = " + s); //결과 32767
System.out.println("n = " + n); //결과 550
-----------------------------------------------------------------------------
실수형(소수)
float f = 3.14; //java에서 실수는 기본적으로 double형으로 인식하기 때문에 float자료형을 	 //사용한다는 것을 명시해줘야 한다. (3.14f)

float f1, f2;
f1 = 3.14f;
f2 = 150; //실수에도 정수 대입이 가능. 실수에 정수를 대입하면 자동 실수화 됨. 결과보자

System.out.println("f1 = " + f1); //결과 3.14 
System.out.println("f2 = " + f2); //결과 150.0
```


