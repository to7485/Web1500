## 형변환

### Promotion(자동형변환)
- 작은 자료형을 큰 자료형에다 대입하는 것

```java
   double d = 100.5; //8byte
   int n = 200; //4byte
   d = n;
   System.out.println("d = " + d); //결과 : 200.0

   char c = 'A'; //2byte
   long l = 100;//8byte
   l = c;
   System.out.println("l = " + l); //결과 65
```


### Demotion(강제형변환)
- 큰 자료형을 작은 자료형에다 대입하는 것.
```java
   char c = 'B'; //2byte
   int n = c + 1; //여기까지는 프로모션 캐스팅
   c = n; //c는 2byte, n은 4byte이므로 오류 발생   
   c = (char)n; //이렇게 수정
   System.out.println("c = " + c);

   float f = 5.5f;
   int n = 0;
   n = (int)f; //같은 4byte여도 자료형이 일치하지 않으면 대입되지 않음. 고로 캐스팅
   System.out.println("n = " + n);
   //결과는 5 인데, float에서 int로 캐스팅되면서 소수점 이하 자리를 상실함
```

## 자바의 특징
```java
짚고 넘어갈 자바의 장점(신기함)
byte b1 = 100;
byte b2 = 20;
byte b3 = b1 + b2; //오류남.

int b3 = b1 + b2; //이렇게 수정
```
byte의 표현 범위가 127까지 밖에 되지 않다보니, byte끼리의 연산은 127을 넘어가버릴 가능성이 높다.<br>
이런 상황을 대비하여 java개발자들은 byte끼리의 연산이 수행될 때, int형 변수로 값을 받도록 만듦.<br>

<hr>

## 연산자(Operator)
### operator 패키지 생성

1. 최고연산자 : . , ()
2. 증감연산자 : ++, --
3. 산술연산자 : + , - , * , / , %(모듈러. 나머지 연산자)<br>
    ( 10 / 3 = 3  <-- 요놈은 몫을 구한다. , 
    10 % 3 = 1 <-- 요놈은 나머지를 구한다. )
4. 시프트 연산자 : >> , << , >>>
5. 비교연산자 : > , < , >= , <= , == , !=
6. 비트연산자 : & , | , ^ , ~
7. 논리연산자 : && , || , !
8. 조건(삼항)연산자 : ? , :
9. 대입연산자 : = , *=, /= , %= , += , -=

### 산술연산자
- 산술연산자는 4칙연산(+,-,x,/)과 나머지 값을 구하는 연산자로 나뉜다.

```java
int n1, n2, n3;
n1 = 20; //n1에20을대입
n2 = 7; //n2에7을대입
n3 = n1 + n2; //n1 + n2의값을n3에대입
System.out.println("n3 = " + n3); //결과27

n3 = n1 - n2;
System.out.println("n3 = " + n3); //결과 13

n3 = n1 / n2;
System.out.println("n3 = " + n3); //결과 2 - 몫 출력

n3 = n1 % n2;
System.out.println("n3 = " + n3) //결과 6 - 나머지 출력
```

### 대입연산자
- 첫날부터 가장 많이 사용해봤던 연산자, 항상 우변의 값을 좌변에 대입을 한다 라고 생각하자!
- 복합대입연산자 산술연산자와 대입연산자가 합쳐진 형태, +=,-=,*=,/=,%=
```java
int n1 = 10; //n1이라는 int형 변수에 10이라는 정수를 대입함.
int n2 = 7;
System.out.println("=연산자: n1 = " + n1 + ", n2 = " + n2);


int n3 = 13;
int n4 = 15;
System.out.println("+=연산자: n3 += n4 = " + (n3 += n4)); //n3 = n3 + n4을 줄여서 씀


int n5 = 10;
int n6 = 3;
System.out.println("/=연산자: n5 /= n6 = " + (n5 /= n6)); //n5 = n5 / n6를 줄여서 씀
```

### 비교연산자
- 변수나 상수의 값을 비교하여 참과 거짓을 판단하는 연산자.
- 결과가 항상 true나 false로 반환된다. (반환을 받는다는건 연산식 자체가 반환값 데이터로 바뀌게 된다)

```java
int n1 = 10;
int n2 = 20;
boolean result;
result = n1 < n2;
//한줄로하면boolean result = n1 < n2;
System.out.println("n1 < n2 : " + result);

result = n1 == n2;
System.out.println("n1 == n2 : " + result);

result = n1 != n2;
System.out.println("n1 != n2 : " + result);
```
### 논리연산자
- 비교연산자를 통한 연산이 2개 이상 필요할 때 사용한다.
- &&, ||, !

#### &&은 And의 의미를 가지고 있다. 앞 뒤의 연산이 모두 True여야 True를 반환한다.
```java
int myAge = 30;
int limit = 35;

//&&은 앞쪽의 연산이 false일때 뒤쪽연산을 수행하지 않고 넘어간다.
//&&는and의 뜻. '~하고'라는 의미로 이해하면 도움이 된다.
//참 && 참 = 참
//참 && 거짓 = 거짓
//거짓 && 참 = 거짓
//거짓 && 거짓 = 거짓
//둘 다 참일때만 참
boolean result = (limit - myAge) >= 5 && myAge > 30;
System.out.println("&&연산결과: " + result);
```
#### ||는 Or의 의미를 가지고 있다. 앞 뒤의 연산중 하나만 True이어도 True를 반환한다.

```java
int n1 = 10;
int n2 = 20;
//||은앞쪽의연산이false여도 뒤쪽연산을수행한다.
//||는or의 뜻. '~거나'라는의미로이해하면도움이된다.
//참 || 참 = 참
//참 || 거짓 = 참
//거짓 || 참 = 참
//거짓 || 거짓 = 거짓

//한쪽만 참이어도 참
boolean result2 = (n1 += 10) > 20 || n2 - 10 == 11;
System.out.println("||연산결과: " + result2);
```

#### !는 not의 의미를 가지고 있다. True를 False로 False를 True로 바꿔준다.
```java
//! 는not의 뜻. true는 false로, false는 true로 바꿔서나타낸다.
System.out.println("!연산결과: " + !result2);
```

### 비트연산자.
논리 연산자와 유사하지만 bit단위(2진수)의 연산만 가능하다.<br>
일반적으로 다음에 배울 시프트 연산자와 더불어 암호화, 복호화 작업에 사용되며.<br>
나는 아직까지 실무에서 써본 적이 없다.<br>
그러나 몰라서 못하는 것과, 아는데 안하는 것은 다르니 일단 알고 넘어가자.<br>

```java
int a = 10; //1010
int b = 7; //0111
int c = a & b; //논리곱(and) - 2진수로바꿨을 때 두값이모두1 일때만결과가1. 나머지는0
System.out.println("c : " + c);

int a2 = 12;
int b2 = 8;
int c2 = a2 | b2; //논리합(or) - 2진수로바꿨을 때 두값이모두0일때만결과가0. 나머지는1
System.out.println("c2 : " + c2);

int a3 = 9;
int b3 = 11;
int c3 = a3 ^ b3; //배타적or(xor) - 2진수로바꿨을때 두값이 서로같을때는0.서로다를때는1
System.out.println("c3 : " + c3);  
```

### 증감연산자.
- 1씩 증가시키거나 1씩 감소시키는 연산자.
- ++, --
#### 선행증감
- 변수의 앞에서 사용된다.
```java
int a = 10;
System.out.println("a : " + ++a); //결과 11
```

#### 후행증감
- 변수의 뒤에서 사용된다.
```java
int b = 10;
System.out.println("b : " + b++); //결과 10
//여기까지 일단 결과 보여주고 아래쪽 System.out.println()써서 확인시켜주자.
		
//여기서는 값이 증가되어 있다.
//b++연산을 수행 한뒤 대기하다가 다시 b를 만났을때 증가된 값을 출력했기 때문.
System.out.println("b++ : " + b); //결과 11

//이렇게 보면 쓸모 없어보이는 것 같아도 정말 많이 쓰이는 연산자 중 하나.
//반복문 들어가면 쓸 일 많아진다.
```

### 삼항연산자.
- 하나의 조건을 정의하여 조건식이 참일 때 반환할 명령, 거짓일때 반환할 명령을 얻어내기 위한 연산자.

```java
int a = 10;
int b = 15;
boolean result;		
result = ++a >= b ? true : false;
System.out.println("result :" + result);
		
int n1 = 10;
int n2 = 20;
char result2;
result2 = (n1 += n1) == n2 ? 'O' : 'X';
System.out.println("result2 : " + result2);
//삼항연산의 값을 받을 변수의 자료형과 ?뒤의 결과값의 타입이 같아야 한다.
```

### 연산자 문제
```java
int a = 10;
int b = 12;
//++a >= b || 2 + 7 <= b && 13 - b >= 0 && (a += b) - (a % b) > 10;
오류나므로 주석처리 하고 코드 작성없이 결과 도출해 보라 하자

//풀이
int a = 10;
int b = 12;
boolean result;
result = ++a >= b || 2 + 7 <= b && 13 - b >= 0 && (a += b) - (b % a) > 10;
System.out.println(result); 

결과 = true
-----------------------------------------------------------------------------------------------------
/*
* 과수원이 있다.
* 
* 배, 사과, 오렌지를 키우고 있는데 하루에 생산되는 양은 각각
* 5,  7,   5개.
* 
* 과수원에서 하루에 생산되는 총 개수를 출력하고, 시간당 
* 전체 과일의 평균 생산 갯수를 출력.
* 평균값을 담는 변수는 float으로 할 것.
*/
	
//풀이	
int pear = 5; //배
int apple = 7; //사과
int orange = 5; //오렌지
		
int fruitTotal = pear + apple + orange; //하루생산량
System.out.println("하루에 생산되는 과일 수 : " + fruitTotal + "개");
		
float average = fruitTotal / 24f; //시간당 평균
System.out.println("시간당 평균 생산 갯수 : " + average + "개");
```












