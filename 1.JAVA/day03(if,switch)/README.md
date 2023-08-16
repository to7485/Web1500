## 제어문
- 프로그램의 흐름을 제어하는 문장
  - 조건문 : if,switch
  - 반복문 : for,while, do-while

## if문
```java
기본형
if(조건식){
  조건식이 참일 때 실행할 문장.
}
```

### 단순 if문
```java

int n = 50;
String str = null; 
//String은 처음이지? 일단 똑같이 쓰고 쌍따옴표 안에 문자열 넣는거야
//String의 기본값은 null, String은 기본자료형이 아니라는것만 일러두자.
		
if(n == 50){
    str = "n은 50입니다."; 
}
//괄호안의 값이 true일 경우엔 그 아래쪽 연산을 수행
//괄호안의 값이 false일 경우엔 아래쪽 연산을 수행하지 않음
//괄호안에는 무조건 true나 false의 결과를 가지는 연산이 들어와야 함
		
//위에 결과 보여주고 if추가, n값 51로 변경후 다시 보여주기
if(n != 50){
    str = "n은 50이 아닙니다.";
}		
System.out.println(str);
```

### if - else문
```java
기본형
if(조건식){
  조건식이 참일 때 실행할 문장.
} else {
  조건식이 거짓일 때 실행할 문장.
}
```
```java
if ~ else문 : 

int n = 49;
String str = null;//
		

if(++n >= 50){
    str = "n은 50이상의 수";
}
else{
    str = "n은 50미만의 수";
}
		
System.out.println(str);
```

#### 문제
변수 age에 나이를 대입하고, 30세 이상이면...<br>
“드실만큼 드셨네요”, 그렇지 않으면 “더 드세요”를 출력하는 if문을 구현 한 후<br>
마지막으로 “감사합니다”라는 문장을 출력해보자.<br>

```java
int age = 30;

if(age >= 30){
	System.out.println("드실만큼 드셨네요ㅋ");
}else{	
	System.out.println("좀 더 드셔야겠어요ㅋ");
}
System.out.println("감사합니다.");
```

#### 문제2
위 코드를 삼항연산자로 작성해보세요.
```java
//if-else문을 배운 순간 삼항연산자는 사용하지 않아도 됨
//모든 if – else문은 삼항연산자로 바꿀수 있고 반대로도 됨
//if-else를 썼을 때 보다 코드가 짧아지는 경우가 있어서 알고 있으면 좋다.
age >= 30 ? “드실만큼 드셨군요” : “더 드셔도...”;
```

### if - else if
- 비교해야할 조건이 여러개 있는 경우
```java
기본형
if(조건식1){
  조건식1이 참일 때 실행할 문장.
} else if(조건식2) {
  조건식1이 거짓이고 조건식2가 참일 때 실행할 문장.
} else if(조건식3){
  조건식1,2가 거짓이고 조건식3이 참일 때 실행할 문장.
} else {
  위의 조건이 모두 거짓일 때 실행할 문장
}
```

```java
int num = 75;
	
if(num >= 90)
System.out.println("성적은 A입니다.");

else if(num >= 80)	
System.out.println("성적은 B입니다.");

else if(num >= 70)
System.out.println("성적은 C입니다.");

else if(num >= 60)	
System.out.println("성적은 D입니다.");

else
System.out.println("성적은 F입니다.");
		
```

### if문의 중첩
- 제어문은 자유롭게 중첩해서 사용할 수 있습니다.
- if문 안에 if문이 있는 경우

```java
if(조건식1){
  if(조건식2){
      조건식1,2가 모두 참일 때 실행할 문장 
  }
}
```
```java
//숫자를 하나 입력받아서
//해당 숫자가 5의 배수이면서 홀수이면 "5의 배수이면서 홀수입니다."
//홀수가 아니면 "5의 배수이면서 짝수입니다" 출력
//5의 배수가 아니면 "5의 배수가 아닙니다" 출력하는 프로그램 작성하기
Scanner sc = new Scanner(System.in);

System.out.print("정수를 입려하세요 : ");
int n = sc.nextInt();

if( n % 5 == 0) {
	if(n % 2 != 0) {
		System.out.println("5의 배수이면서 홀수입니다.");
	} else {
		System.out.println("5의 배수이면서 짝수입니다.");
	}
} else {
	System.out.println("5의 배수가 아닙니다.");
}

```
```java
int num = 5;
if(num <=10){
  if(num % 2 == 1){
    System.out.println(num + "은 홀수입니다."); 
  }
}
```

## switch문
if문과 비슷하지만 if문은 괄호안에 인자값이 true, 혹은 false로 결정되는 조건식이 들어가야 하고<br>
Switch문은 인자값으로 조건이 아닌 비교할 값이 들어가야 한다.<br>
특정 값을 바로 찾아 들어가기 때문에 if문에 비해 처리속도가 빠르다.<br>
```java
기본형
switch(비교값){
case 조건값 :
    비교값과 조건값이 일치할 때 실행할 문장.
    break;
case 조건값 :
    비교값과 조건값이 일치할 때 실행할 문장.
    break;
case 조건값 :
    비교값과 조건값이 일치할 때 실행할 문장.
    break;
}
```
```java
//1) 비교값과 조건값의 타입은 반드시 일치해야 한다.
//2) 중복되는 조건값을 가질 수 없다.
int n = 1;
	
switch (n) {//인자로 비교할 값이 들어와야 한다.
case 1://인자와 비교할 조건값이 들어온다.
	System.out.println("1. 게임하기");	
	break;//현재 switch문을 빠져나오는 키워드.

case 2:	
	System.out.println("2. 게임소개");	
	break;	
case 3:	
	System.out.println("3. 종료");	
	break;
	
default://비교값과 조건값이 일치하는 것이 하나도 없는 경우 반드시 실행되는 영역			
    System.out.println("메뉴선택이 올바르지 않습니다.");	
	break;
}
```
## if vs switch
둘 다 조건에 따라서 명령을 실행하는 것은 맞지만<br>
if문은 범위에 따라서 조건을 비교하는데 효과적이고<br>
switch문은 하나의 값에 따라서 조건을 비교하는데 효과적이다.<br>

```java
char c = ‘A’;

switch (c) { //인자로 비교할 값이 들어와야 한다.

case A://인자와 비교할 조건값이 들어온다.	
	result = "90 ~ 100점";	
	break;
	
case B://콜론이다. 세미콜론 아니다.	
	result = "80 ~ 89점";	
	break;
	
case C:	
	result = "70 ~ 79점";	
	break;
	
case D:	
	result = "60 ~ 69점";	
	break;
	
case F:	
	result = "59점 이하";	
	break;
	
default:	
	result = "제대로 입력하시지";	
	break;
}
```
```java
//switch문의 비교값으로 사용 가능한 자료형
//1) 정수(byte,short,int)
//2) 문자형(char)
//3) 문자열(String)

String str = "홍";
String result;
	
switch (str) { //인자로 비교할 값이 들어와야 한다.

case "박“://인자와 비교할 조건값이 들어온다.	
result = "박길동";	
break;
	
case "이"://콜론이다. 세미콜론 아니다.	
result = "이길동";	
break;
	
case ”독고":	
result = "독고길동";	
break;
	
case “홍":	
result = "홍길동";	
break;
	
default:	
result = "제대로 입력하시지";	
break;
}
	
System.out.println(result);
```

#### 정수형 변수를 하나 만들고 해당 달이 몇일까지 있는지 switch문을 이용해서 작성하시오.
```java
int month = 4
switch(month) {
case1: case3: case5:
case7: case8: case10:
case12:
  System.out.println( month + “월은 31일 까지 있습니다.”);
  break;
case4: case6:
case9: case11:
  System.out.println( month + “월은 30일 까지 있습니다.”);
  break;
case2:
  System.out.println( month + “월은 28일 까지 있습니다.”);
}
```
#### 두개의 정수형 변수를 초기화 한다 (값은 자유), 그리고 연산자를 담아줄 문자열 변수를 만든다. switch문을 이용하여 정수의 연산을 수행하는 코드를 작성해보자.
```java

실행결과 :
10 * 7 = 70
풀이 :
int su1 = 10;
int su2; = 7;
String op = "*";
	
switch (op) {

case "+":	System.out.println(num1 + " + " + num2 + " = " + (num1 + num2));
//(num1 + num2)괄호로 안묶으면 결과값이 더해지지 않고 문자열로 붙어서 나온다.	
    break;

case "-":	System.out.println(num1 + " - " + num2 + " = " + (num1 – num2));	
    break;
	
case "*":	System.out.println(num1 + " * " + num2 + " = " + (num1 * num2));	
    break;
	
case "/":	System.out.println(num1 + " / " + num2 + " = " + (num1 / num2));	
    break;
	
default:	result = "올바른 연산자가 아닙니다.";	
    break;
}	
```





