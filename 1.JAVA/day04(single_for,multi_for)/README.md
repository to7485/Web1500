## 반복문
### 특정 수행문을 여러번 반복할 수 있도록 해주는 제어문

반복문: 특정수행문을 원하는만큼 반복하여 실행하는 제어문<br>
for문 : 특정 명령을 원하는 만큼 반복적으로 처리할 때 사용한다.<br>

```java
for(초기식; 조건식; 증감식){
	조건식에 해당할때 반복할 명령
}
```
초기식 : 반복을 하기 위한 시작값으로 변수를 하나 초기화 한다.<br>
조건식 : 반복을 하기 위한 종료값으로 비교연산자를 많이 사용한다.<br>
증감식 : 변수의 값을 증감시켜주는 역할을 한다 증감연산자를 많이 사용한다.<br>
<br>
※후행증감이나 선행증감이나 영향은 없지만 개발자들이 다들 후행증감을 사용하므로 후행증감으로 사용하도록 하자.<br>
<br>


```java
for(int i = 0; i <= 3; i++){
	System.out.println(i);
}//for문 돌아가는 구조 설명해주기.
```
![image](https://user-images.githubusercontent.com/54658614/215010183-dceb4e66-811e-46ba-a8b6-e3e902446b3d.png)

```java
문제 : 1부터 10까지 반복하는 for문을 작성해보자.
for(int i = 1; i <= 10; i++){
	System.out.println(i);
}

문제 : 10부터 1까지 감소하는 for문을 작성해보자.
for(int i = 10; i >= 1; I--){
	System.out.println(i);
}

문제 - 1부터 10까지 반복하는 문장에서 3의 배수만 출력하는 제어력을 갖추어 보자!	
for(int i=1; i<=10 ; i++){
	if(i%3 == 0){
	    System.out.println(i + “은(는) 3의 배수입니다.”);
	}
}//for의 끝!


문제 - 1부터 20까지 홀수를 차례대로 출력해보자.
for(int i=1; i<=19; i++) {
        if(i%2!=0) {
             System.out.print(i+" ");
        } //if
     } //for

 
문제 - 1부터 10까지 총 합을 구해보자.
int total = 0;
for(int i = 1; i<=10; i++){
 	total += i;
}
System.out.println("1~10까지의 총합 : " + total);
 
정수형 변수를 하나 초기화 하고 해당 정수에 해당하는 구구단 출력하기
int dan = 2;
for(int i = 1; i <= 9; i++){
	System.out.println(dan + " X " + i + " = " + (dan * i));
}
```
## 다중 for문
### for문 안에 for문이 있는 경우

```java
for(초기식;조건식;증감식){
	for(초기식;조건식;증감식){
		
	}
}

```
다중 for문이 작동하는 원리

```java
for(int i = 1;i<=3;i++){
	for(int j = 1;j<=3;j++){
		System.out.println(i+" " + j);
	}
	System.out.println();
}

```
### 결과
![image](https://user-images.githubusercontent.com/54658614/215011017-deb1c912-68c8-416b-b3c9-9e4a7b965e25.png)

바깥쪽 for문이 1바퀴 도는동안 안쪽 for문은 완전히 반복을 한다.<br>
안쪽 for문이 끝나게 되면 밑에 줄바꿈용 출력문을 실행하고 바깥쪽 for문의 증감식으로 올라간다.<br>

```java

//    1 2 3 4
//    1 2 3 4
//    1 2 3 4

for(int i = 1; i <= 3; i++){ //y축	         
	for(int j = 1; j < 5; j++){ //x축
		System.out.print(" " + j);	
     }//안쪽 for문의 끝
	System.out.println();//줄바꿈

}// 바깥쪽 for문의 끝

----------------------------------------------------------------예제 1

// 01 02 03 04
// 05 06 07 08
// 09 10 11 12

int count = 0;
	
for(int i = 1; i <= 3; i++){ //y축
     for(int j = 1; j < 5; j++){ //x축	              
	System.out.printf("%02d ",++count);// 정수3자리에 j값 출력!
     }//안쪽 for문의 끝	           
     System.out.println();//줄바꿈
}// 바깥쪽 for문의 끝
----------------------------------------------------------------예제ex

// A B C D
// E F G H
// I J K  L
char ch = 'A';

for(int i = 1; i <= 3; i++){	          
	for(int j = 1; j <= 4 ; j++){
    System.out.print(" " + ch++);
     }//System.out.printf("%2c", ch++);
    System.out.println();//줄바꿈
}
------------------------------------------------------------------ 예제 2


//* 
//* * 
//* * * 
//* * * * 
//* * * * *

for(int i = 1; i <= 5; i++){	          
	for(int j = 0; j < i; j++){
	    System.out.print("* ");
    	}	
	System.out.println();
     }         
}


------------------------------------------------------------------ 예제 3

//        * 
//      * *
//    * * *
//  * * * *
//* * * * *

for(int I = 0; i< 5; I++) {
   for(int j = 0; j< 5 - (i+1); j++){
       System.out.print(“ ”);
   }//inner for1
    for(int k = 1; k <= I+1; k++) {
        System.out.print(“*”);
   }//inner for2
   System.out.println();
}//outer for

------------------------------------------------------------------ 예제 5
다중for문을 이용하여 아래와 같은 결과를 도출하는 로직을 구현하자.

결과)
1 2 3 4 5 6 7 8 9 10 
2 3 4 5 6 7 8 9 10 1 
3 4 5 6 7 8 9 10 1 2 
4 5 6 7 8 9 10 1 2 3 
5 6 7 8 9 10 1 2 3 4 
6 7 8 9 10 1 2 3 4 5 
7 8 9 10 1 2 3 4 5 6 
8 9 10 1 2 3 4 5 6 7 
9 10 1 2 3 4 5 6 7 8 
10 1 2 3 4 5 6 7 8 9

풀이)
public class Work {
	public static void main(String[] args) {
	for(int i = 1; i <= 10; i++){	          
		for(int j = 0; j < 10; j++){

		     int num = i + j;
	             if(num > 10){
		            num -= 10;
            
	             }//if문
        	System.out.print(num + " ");		          
		}//안쪽 for문
	 System.out.println();
	 }//바깥쪽 for문
    }//main
}

```
