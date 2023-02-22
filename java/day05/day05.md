## while문
### 간편한 구성을 가진 반복문(선비교 후처리)

```java
while(조건식){
    조건식이 참일때 반복할 명령
}
```
while문은 for문처럼 시작값과 증감값을 가지고 있지 않기 때문에 값을 변화시켜주지 않으면 무한반복이 일어나게된다.
```java
int num = 1;
		
while(num <= 10){ 
	System.out.println(num);
}//while문의 끝
```
```java
int num = 1;
		
while(num <= 10){ 
	System.out.println(num);
	num++;
}//while문의 끝
```
## do-while문
while문은 선비교 후처리를 하지만<br>
do-while문은 선처리 후비교를 하기 때문에 조건에 맞지않아도 무조건 한번은 실행을 하게 된다.<br>
제어문 중 유일하게 뒤에 세미콜론(;)을 붙혀야 한다.<br>

```java	
int i = 11;
do{
    System.out.println(i);

}while(i <= 10); //결과 : 11
```
## 기타제어문
### break
Ex1_break 클래스 생성<br>
반복문 내에서 break를 만나게 되면 가장 가까운 반복문을 아예 빠져나가 다음 코드를 실행하게 된다.<br>
```java
for(int i = 1; i <= 2; i++){ 
	             for(int j = 1; j <= 5; j++){ 
	
         if(j % 2 == 0){//j 를 2으로 나눈 나머지가 0일때
          break;// 가장 가까운 반복문 탈출!
         }
	          System.out.print(" " + j);
    }//안쪽 for문의 끝
	System.out.println();//줄 바꿈!
}//바깥쪽 for문의 끝
```
Ex2_break 클래스 생성<br>
```java
int n = 1;
	
while (true) {//무한반복. !true나 false는 불허
	System.out.println(n);
n++;
	
//n이 10보다 클 때 break로 while문을 빠져나오는 구문을 만들어보자	

if(n > 5){
	break;
  }
}
```
### continue
### ex_continue 패키지 생성하기
Ex1_continue 클래스 생성<br>
반복문 내에서 continue를 만나게 되면 가장 가까운 반복문의 증감식으로 돌아간다.(증감식이 없을 때 조건식으로 돌아간다)
```java
public class Ex1_continue {
	public static void main(String[] args) {
		
		//continue문 : 반복문 내에서 특정 문장을 건너뛰고자 할 때 사용
		
		for( int i = 1; i <= 2; i++) {
			
			for(int j = 1; j <=5;) {
				
				if(j % 2 == 0) {
				//가장 가까운 반복문의 증감식으로 이동, 증감식이 없다면 조건식으로 이동
					continue;
				}
				j++;
				
				System.out.print(j + " ");
			}//inner
			System.out.println();
		}//outer
		
	}//main
}
```
Ex2_continue 클래스 생성<br>
```java
public class Ex2_continue {
	public static void main(String[] args) {
		
		int n = 0;
		
		while(n < 10) {
			n++;
			
			if( n % 2 != 0) {
				//while문에서 continue를 만나면 조건식으로 이동
				continue;
			}
			
			System.out.println(n);
		}//while
		
```
### label
break,continue에 라벨을 달아서 가장 가까이 있는 반복문이 아닌 내가 원하는 반복문을 빠져나올수 있게 한다.
```java
Ex3_label_break 클래스 생성
		//label: 특정 반복문에 이름표를 붙여 두 개 이상의 반복문을
		//       제어할 수 있도록 하는 키워드
		
		//label은 항상 쌍으로 존재한다.
		//label의 이름은 자기가 원하는대로 사용이 가능하다.
		
		//label은 자신을 포함하고 있는 상위 개념에게만 달 수 있다.

		happy: for (int i = 1; i <= 3; i++) {

			for (int k = 1; k <= 10; k++) {
				System.out.println(k + " ");
			}

			for (int j = 1; j <= 10; j++) {

				if (j % 2 == 0) {
					break happy;
				}
				System.out.print(j + " ");
			} // inner
			System.out.println();
		} // outer
}

--------------------------------------------------------------------예제1
continue 패키지에 클래스 만들기
Ex3_label_continue 클래스 생성
int n = 0;
		
outer : while(true){
	
if( n >= 10) //이 부분은 무한반복 되는거 확인후 가장 마지막에 추가.	
		break;
	while(true){
	
n++;		
if(n % 3 == 0){
	System.out.println("continue를 만남");
	continue outer;	System.out.println(n);
	
}
--------------------------------------------------------------------------------
n = 0;
		
		w : while( n < 10) {
			
			n++;
			
			switch( n ) {
			case 1:
				System.out.println("switch문 1번 거쳐감");
				//switch문에서 break는 반복문을 나가는게 아닌 switch문만 나가게 된다.
				break w;
				
			case 2:
				System.out.println("switch문 2번 거쳐감");
				//switch문만 단독으로 사용을 할 때는 continue를 사용할 수 없다.
				continue;
			
			}//switch
			System.out.println(n);
			
		}//while

```
## printf
문자열과 변수에 들어있는 데이터를 함께 출력할 수 있도록 도와주는 포맷형식<br>
printf의 f는 format이라는 뜻이다.<br>
1. %d : 정수형을 입력받을 때 사용한다.
2. %c : 문자형을 입력받을 때 사용한다.
3. %s : 문자열을 입력받을 때 사용한다.
4. %f : 실수형을 입력받을 때 사용한다.

```java
int su1 = 10;
int su2 = 7;
		
System.out.println(su1 + " + " + su2 + "=" +(su1+ su2));

System.out.printf("%d + %02d = %d\n", su1,su2,(su1+su2));

int age = 30;
//저의 나이는 30세 입니다.
System.out.printf("저의 나이는 %d세 입니다.\n", age);
		
//저의 나이는 30세이고 키는 170입니다.
System.out.printf("저의 나이는 %d세 이고 키는 %d입니다.\n", age, 170);
		
//저의 혈액형은 AB형입니다.
System.out.printf("저의 혈액형은 %s형입니다.\n", "AB");
		
//원주율은 3.141592입니다.
System.out.printf("원주율은 %.2f입니다.\n", 3.141592);

System.out.println("---------------------------------------");
		
// 01 02 03 04
// 05 06 07 08
// 09 10 11 12
		
int count = 1;
for(int i  = 0; i<3; i++) {
			
    for(int j = 0; j <4; j++) {
			System.out.printf("%03d ", count++);
    }//inner
		System.out.println();
}//outter
```


## Scanner
키보드에서 값을 직접 입력받아 변수에 저장할 수 있도록 해주는 클래스<br>
```java
Scanner 객체명 = new Scanner(System.in);
```

```java
		System.out.print("나이: ");
		
		//키보드에서 int타입의 값을 받고 엔터를 치는 순간
		//num변수에 사용자가 입력받은 값을 대입을 해준다.
		int num = sc.nextInt();
		
		System.out.printf("제 나이는 %d 살 입니다.\n",num);
		System.out.println();
		System.out.print("이름: ");
		String name = sc.next();
		System.out.printf("제 이름은 %s 입니다.\n",name);
		System.out.println("문장 : ");
		String s1 = sc.next();
		System.out.println("받은 문장 : " + s1);
		
		System.out.println("---------------------------------------");
		//키보드에서 값을 입력받고 해당하는 구구단을 출력 해보세요.
		//2~9 사이의 숫자만 입력하세요
		
		System.out.print("단 입력: ");
		int dan = sc.nextInt();
		
		if(dan <= 1 || dan >=10) {
			System.out.println("2~9 사이의 숫자를 입력하세요.");
		} else {
			for( int i = 1; i <= 9; i++) {
				System.out.printf("%d * %d = %d\n",dan, i, (dan * i));
			}//for
		}
		 	
	}//main
}
```
## for vs while
for문은 내가 몇번 반복해야하는지 정확하게 알 때 사용을 한다.<br>
while문은 몇번 반복해야하는지 정확하게 알 수 없을때도 사용이 가능하다<br>

아래의 예제는 사용자가 -1을 입력하는 시기를 예측할 수 없습니다.
```java
Scanner sc = new Scanner(System.in);
		
System.out.print("정수를 입력해주세요 : ");
int num = sc.nextInt();
		
while(num != -1) {
	System.out.printf("입력한 정수 : %d\n",num);
	System.out.print("정수를 입력해주세요 : ");
	num = sc.nextInt();
	if(num == -1) {
		System.out.print("-1이 입력되어 종료합니다.");
		break;
	}
			
}
```
<hr>
## 예제

```java
자바 1주차(2) 문제 - 1
Scanner를 통해 정수 n을 입력받는다.
그리고 1부터 입력받은 정수 n까지의 합을 계산하여 그 결과를 출력하는 프로그램을 작성.
예를들어 정수 5를 입력받으면, 1 + 2 + 3 + 4 + 5의 연산결과인 15를 출력해야 한다.

public class Work_Ex1 {
	public static void main(String[] args) {
		
int result = 0;
System.out.print("숫자입력 : ");
Scanner scan = new Scanner(System.in);	
int n = scan.nextInt();
	for(int i = 1; i <= n; i++){
		result += i;
		System.out.println("결과 : " + result);
  	}
}//main


자바 1주차(2) 문제 - 2
Scanner를 통해 정수 n1, n2를 입력받는다.
그리고 n1부터 n2까지의 합을 계산하여 그 결과를 출력하는 프로그램을 작성.
예를들어 2와 5를 입력받으면, 2 + 3 + 4 + 5의 연산결과인 14를 출력해야 한다.

public class Work_Ex2 {	
	public static void main(String[] args) {
		
	int n1 = 0, n2 = 0;
	int result = 0;
		
	System.out.print("첫번째 숫자입력 : ");	
	Scanner scan = new Scanner(System.in);	
	n1 = scan.nextInt();
	System.out.print("두번째 숫자입력 : ");
	n2 = scan.nextInt();
		
	//혹시 스왑을 사용하고 싶다면...	
	if(n1 > n2){
	    int n3 = n1;
	    n1 = n2;
	    n2 = n3;
	}

	for(int i = n1; i <= n2; i++){	
		result += i;
	}
	System.out.println("결과 : " + result);
    }
}

자바 1주차(2) 문제 - 3
키보드에서 숫자를 두 개 입력 받아, 입력받은 두 수의 최소공배수 구하기.
예를들어 2와 5를 입력받았다면 10을, 
         3과 3을 입력받았다면 3이 출력되어야 한다.

public class Work_Ex3 {
	public static void main(String[] args) {
		
		int n1 = 0, n2 = 0;
		
		System.out.print("첫번째 숫자입력 : ");
		Scanner scan = new Scanner(System.in);
		n1 = scan.nextInt();
		
		System.out.print("두번째 숫자입력 : ");
		n2 = scan.nextInt();
		
		int i = 0;
		for(i = 1; i <= n1 * n2; i++){
			
			if(i % n1 == 0 && i % n2 == 0)
				break;						
		}
		//while(true){
			//i++;
			//if(i % n1 == 0 && i % n2 == 0)
				//break;
		//}

		//int i = 1;	
		//while(!(i % n1 == 0 && i % n2 == 0)){
			//i++;
		//}
		System.out.println("최소공배수는 " + i);
	}
}


자바 1주차(2) 문제 - 4
키보드에서 숫자를 두 개 입력 받아, 입력받은 두 수의 최대공약수 구하기.
예를들어 10과 4를 입력받았다면 2가,3과 7을 입력받았다면 “최대공약수가 없습니다”라는 문자열이 출력 되어야 한다.

public class Work_Ex4 {
	public static void main(String[] args) {

int n1 = 0, n2 = 0;
int check = 0;
	System.out.print("첫번째 숫자입력 : ");	Scanner scan = new Scanner(System.in);
n1 = scan.nextInt();

		System.out.print("두번째 숫자입력 : ");
		n2 = scan.nextInt();

		if(n1 >= n2)
			check = n2;
		else
			check = n1;

		int i;
		for (i = check; i >= 1; i--) {
			if ((n1 % i == 0) && (n2 % i == 0)) {
				break;
			}
		}
		
		if (i == 1) {//두 숫자에는 1이외의 공통된 약수가 없다는 의미.
			System.out.print("최대공약수가 없습니다.");	
		}else {
			System.out.print("최대공약수 : " + i);
		}
	}
}

자바 1주차(2) 문제 – 5
키보드에서 숫자를 입력받으면 그 숫자가 소수인지 아닌지를 판별해주는 코드 작성.

public class Work_Ex5 {
	public static void main(String[] args) {

		int n = 0;
		
		System.out.println("숫자를 입력하세요 : ");
		Scanner scan = new Scanner(System.in);
		
		n = scan.nextInt();
		
		int i = 0;
		for(i = 2; i <= n; i++){
			
			if(n % i == 0){
				break;
			}
		}
		
		if(i == n)
			System.out.println(n + "은(는) 소수입니다.");
		else{
			System.out.println(n + "은(는) 소수가 아닙니다.");
		}
	}
}
```



