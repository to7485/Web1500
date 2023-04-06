# DataBase

## DB(DataBase) 
- 구조화된 정보, 데이터의 조직화된 모음, 일반적으로 컴퓨터에 전자적으로 저장이 되고 데이터베이스관리시스템에 의해 제어된다.

## DBMS(DataBaseManagementSystem)
- 데이터베이스와 사용자 또는 프로그램 간의 매개체 역할을 하여 사용자가 정보의 구성,검색,수정,삭제와 같은 관리를 할 수 있게 해줍니다.

## DBMS의 종류
- 수업을 진행하면서 Oracle을 사용할 것이지만 다양한 DBMS의 종류가 있습니다.

### 인기있는 DBMS 소프트웨어
- MySQL
- Microsoft Access
- Microsoft SQL Server
- FileMaker Pro
- Oracle Database
등등

회사에 가서 다른 DBMS를 사용할 수 도 있지만 프로그래밍언어 처럼 한가지를 배워놓으면<br>
다른 DBMS도 금방 습득할 수 있다.

## 오라클 DBMS 설치
https://www.oracle.com/index.html

- 돋보기 버튼 누르고  XE Prior Release Archive 입력하고 검색하기
- XE Prior Release Archive 클릭하기

![20230406_134017](https://user-images.githubusercontent.com/54658614/230273656-8c50ea22-68c2-49e7-8f39-fdc8de10be0a.png)

- 운영체제에 맞는 버전 다운로드 하기

![image](https://user-images.githubusercontent.com/54658614/230274134-c09aeefb-0b7d-4241-90c9-7a709fa389b7.png)

- 압축 풀고 폴더 안에 있는 setup.exe 실행하기

![image](https://user-images.githubusercontent.com/54658614/230274775-60330aba-0bed-4f43-a7f2-aaa557ec13c6.png)

- 동의 눌러주고 next 진행한다.

![image](https://user-images.githubusercontent.com/54658614/230274935-47d28b33-9579-414b-ad1d-e12f8aec893a.png)

- 설치할 경로를 정해줄 수 있는데 따로 바꿔줄 필요는 없다.(항상 필요한 용량은 확보해두자)

![image](https://user-images.githubusercontent.com/54658614/230275196-3d0c701d-3d0c-4084-a4c0-bc6ebc85310e.png)

- 혹시 DBMS를 지웠다가 다시 까는 경우 이러한 문구가 뜰 수 있다. 

![image](https://user-images.githubusercontent.com/54658614/230275283-67444bb5-02db-4007-b56c-d202bbd3026b.png)

- 설치했던 곳에 있는 폴더를 제거 하고 진행해주자

![image](https://user-images.githubusercontent.com/54658614/230275348-6f5ea512-ad8b-47dc-8e15-074a42e51cca.png)

- DB에 접속할 계정의 비밀번호를 설정해야 한다. 까먹지 않을 비밀번호로 설정하자.

![image](https://user-images.githubusercontent.com/54658614/230275461-9b9e5730-b1e6-44b9-8f82-1060c12a32d0.png)

- 그리고 쭉 진행해준다.

- 설치를 모두 마치면 다음과 아이콘을 더블클릭하여 실행할 수 있다.

![image](https://user-images.githubusercontent.com/54658614/230275813-e86781f6-4251-4534-b196-2e2750e2221e.png)

- 실행을 하면 다음과 같은 화면이 나오는데 실행이 안될 수 도 있다.

![image](https://user-images.githubusercontent.com/54658614/230275882-c3a1fbe5-f768-4eae-bc7e-86ae8cd550f0.png)

- 이러한 오류가 발생할 수 있다.

![image](https://user-images.githubusercontent.com/54658614/230276206-104cb7ae-9bf5-40b9-97d1-9281b08cbac7.png)


![image](https://user-images.githubusercontent.com/54658614/230276002-0da8350c-ec68-429f-8cf1-8afd45647ba2.png)

![image](https://user-images.githubusercontent.com/54658614/230276054-7835d3f8-dc93-435f-a0a1-5231886b99a0.png)

- 이렇게 바꿔주자

![image](https://user-images.githubusercontent.com/54658614/230276265-885421ae-af8b-4092-ab5d-d793754b2657.png)



## SQL문
- 원하는 결과를 얻어오기 위해 DB에 요청하는 요청문장(Query문)
- SQL문장은 대소문자를 구별하지 않는다.
- 한줄 또는 여러줄에 걸쳐 입력하는 것이 가능
- SQL문장의 끝은 세미콜론(;)으로 맺어야 한다. 

## SQL문장의 종류
1. DML(Data Manipulation Language) : 데이터 조작어 사실상 가장 많이 사용하는 쿼리문이 될것! 
    - select, insert, update, delete를 가지는 문장
2. DDL(Data Definition Language) : 데이터 정의어
    - create, alter, drop등의 키워드를 가지는 문장
3. DCL(Data Controll Language) : 데이터 제어어
    - grant, revoke등의 키워드를 가지는 문장

데이터베이스에서는 4대 쿼리라고 불릴 정도로 중요한 개념 뭐가 dml 이고 뭐가 ddl 이고 세세하게 외울 필요는 없고 각각의<br>
키워드를 만났을 때 어떻게 사용하는지만 알면 된다.<br>

 C : Create,insert<br>
 R : select(Read)<br>
 U : update(수정)<br>
 D : delete(삭제)<br>
 
 <hr>
 
 ### SELECT문
 - 데이터베이스에 저장되어 있는 자원들을 검색(조회)할 때 사용하는 문장

```
SELECT 컬럼명1,컬럼명2... FROM  테이블명;
```

```
예) employees(사원)테이블의 모든 정보를 조회하시오

select * from employees; 

예) departments(부서)테이블의 모든 정보를 조회하시오

select * from departments;

예) 사원테이블에서 first_name(이름),job_id(직종),salary(급여)를 조회해보자!

select first_name, job_id, salary from employees;

정보가 많고 컬럼이 많아도 나한테 필요한것만 골라낼수 있기 때문에 속도가 빠르다 자바로 한다고 생각하면 if-while-반복문
돌리면서 찾아줘야 되는데 db는 맞는 정보를 찾아서 바로 화면에 뿌려주는 구조이기 때문에 많은양의 데이터를 관리하는데는
데이터베이스 만한게 없더라

스키마 작성 사원 테이블에서 컬럼이 영어로 되어있기 때문에 뭐라고 되어있는지 한눈에 보기 힘들다 그래서 메모장에다 작성을
하나 한다. 사원테이블이 갖고 있는 컬럼들이 각각 어떤 정보를 저장하기 위해서 만들어져있는지 컬럼들인지를 한번에 정리를 할거다

나중에 문제를 내드리면 이 스키마를 참고 하면서 작성 해보시면 됩니다

문) 사원테이블에서 사번, 이름,입사일,급여를 검색하시오
SELECT employee_id, first_name, hire_date, salary FROM employees;


컬럼에 없는 정보를 출력하고 싶을 때가 있다

예) 사원테이블에서 사번, 이름, 직종, 급여, 보너스, 실제의 보너스 금액을 출력

select employee_id, first_name, job_id, salary, commission_pct,
-- *를 통한 연산을 수행
 salary*commission_pct as comm //as를 빼고 한칸 띄우고 써도 별칭으로 써진다.
 from employees;

실제로 컬럼에는 없지만 간단한 연산을 통하여 컬럼에 존재하지 않는 내용도 조회하는 것이 가능하다~
컬럼에 없던 것이기 때문에 컬럼에 연산식이 그대로 나온다. 연산식이 길어질수록 보기 않좋기 때분에 별칭을 써준다.

SELECT comm from employees; 로 별칭으로 조회가 가능해진다.
```

### where 절(조건부여)
- 사용자가 원하는 자원을 검색할때 조건을 통해 결과를 간추릴 수 있다.

#### WHERE 절의 조건식은 아래 내용으로 구성된다.
- 컬럼명(보통 조건식의 좌측에 위치)
- 비교 연산자
- 문자,숫자,표현식(보통 조건식의 우측에 위치)
- 비교 칼럼명(JOIN 사용시)

#### 조건식 : 참 또는 거짓 둘 중 하나의 결과가 나오는 식
<b>WHERE 조건식</b> 형태로 많이 사용이 된다.<br>
- \> , < : 초과, 미만
- \>=, <= : 이상, 이하
- = : 같다
- < >, !=, ^= : 같지않다.
- AND : 두 조건식이 모두 참이면 참
- OR : 둘 중 하나라도 참이면 참

```
예) 사원테이블에서 급여가 10000이상인 사원들의 정보를 사번, 이름, 급여 순으로 출력

select employee_id, first_name, salary from employees where salary >=10000;

where절은 from 뒤에 있음

예) where절에 문자를 비교하고 싶다면
where first_name='Michael';
*==이 아님 주의, 반드시 홑따옴표로 쓴다.

예) 사원테이블에서 이름이 Michael인 사원의 사번, 이름을 조회
SELECT employee_id, first_name FROM employees WHERE first_name='Michael';

문제) 사원테이블에서 직종이 IT_PROG인 사원들의 정보를 사번, 이름, 직종, 급여 순으로 출력

select employee_id,first_name,job_id,salary from employees where job_id='IT_PROG';

예) 사원테이블에서 급여가 10000이상이면서 13000이하인 사원의 정보를 이름, 급여 순으로 조회
안타깝게도 데이터베이스에서는 && 나 ||를 쓸수가 없다 and , or로 작성을 해야한다.
SELECT first_name, salary FROM employees WHERE salary >= 10000 AND salary <= 13000;


문제) 사원테이블에서 입사일이 05년9월21일인 사원의 정보를 사번, 이름, 입사일 순으로 출력

select employee_id, first_name, hire_date from employees where hire_date='09/21/2005';


예) 사원테이블에서 입사일이 05년9월21일 이후에 입사한 사원의 정보를 사번, 이름, 입사일 순으로 출력

select employee_id, first_name, hire_date from employees where hire_date >='09/21/2005';


데이터베이스는 날짜의 크기비교가 가능하다.

예) 사원테이블에서 06년도에 입사한 사원들의 정보를 사번, 이름, 직종, 입사일 순으로 출력

select employee_id, first_name, job_id, hire_date from employees where hire_date>='01/01/2006' 
and hire_date<='12/31/2006'

문제) 사원테이블에서 급여가 4000~ 8000사이의 사원의 이름,직종,급여를 출력
select first_name, job_id, salary from employees where salary >=4000 and salary<=8000;


문제) 사원테이블에서 직종이 SA_MAN이거나 IT_PROG인 사원들의 모든정보를 출력하시
select * from employees where job_id = 'SA_MAN' or job_id='IT_PROG';


문제) 사워테이블에서 급여가 2200, 3200, 5000, 6800를 받는 사원들의 정보를 사번, 이름 직종, 급여 순으로 출력
select employee_id, fist_name, job_id, salary 
from employees 
where salary='2200' or salary = '3200' or salary='5000' or salary='6800';
```
### SQL연산자
1. BETWEEN : A와 B사이의 값을 조회할 때 사용
2. IN : OR을 대신해서 사용하는 연산자
3. LIKE : 유사검색

```
--BETWEEN연산자 AND연산자를 대체함
예) 사원테이블에서 06년도에 입사한 사원들의 정보를 사번, 이름, 직종, 입사일 순으로 출력
select employee_id, first_name, job_id, hire_date from employees where hire_date between'01/01/2006' and '12/31/2006'; 날짜는 문자열로 인식함

문제) 사원테이블에서 급여가 5000이상이고 6000이하인 사원의 정보를 사번, 이름, 급여순으로 출력
select employee_id, first_name, salary from employees where salary between 5000 and 6000;


--IN연산자
예) 직종이 SA_MAN , IT_PROG인 사원들의 정보를 이름, 직종순으로 출력
select first_name, job_id from employees where job_id IN ('SA_MAN','IT_PROG');

문제) 급여가 2200, 3200, 5000을 받는 사원들의 정보를 이름, 직종, 급여순으로 출력하되 in연산자를 사용하시오

SELECT first_name,job_id,salary from employees WHERE salary=2200 or salary=3200 or salary=5000
select first_name,job_id,salary from employees where salary in (2200,3200,5000);


예) 직종이 'SA_MAN', 'IT_PROG'가 아닌 모든 사원들의 정보를 출력
select * from employees where job_id not in('SA_MAN','IT_PROG');

--LIKE연산자(유사검색)
-- % : 모든값
-- _ : 하나의 값
ex)'M%' : M으로 시작하는 모든 값
ex)'%a' : a로 끝나는 모든 값
ex)'%a%' : 값의 어디든 a를 포함하고 있는 모든 값
ex)'M_ _ _ _' : M으로 시작하는 값들 중 전체 길이가 5글자인 값

예)사원테이블에서 사원들의 이름 중 M으로 시작하는 사원의 정보를 사번, 이름, 직종 순으로 출력
select employee_id, first_name, job_id from employees where first_name LIKE 'M%';
%는 뒤로는 뭐가 몇글자가 오든 상관 안하겠다는 의미

문)사원테이블에서 이름이 d로 끝나는 사원의 사번, 이름 직종을 출력
select employee_id ,first_name, job_id from employees where first_name LIKE'%d';

예)이름의 어디라도 a가 포함되어 있는 사원의 정보를 이름, 직종 순으로 출력
select first_name, job_id from employees where first_name LIKE'%a%';

예)이름의 첫글자가 M이면서 총 7글자의 이름을 가진 사원의 정보를 사번, 이름순으로 검색
select employee_id, first_name from employees where first_name like 'M______'

문제) 사원테이블에서 이름의 세번째 글자에 a가들어가는 사원들의 정보를 사번, 이름 순으로 출력
select employee_id, first_name from employees where first_name like '_ _ a%';

문제) 이름에 소문자o가 들어가면서 이름이 a로 끝나는 사원들의 정보를 이름, 급여 순으로 조회
select first_name, salary from employees where first_name like '%o%a';

문제) 이름이 H로 시작하면서 6글자 이상인 사원들의 정보를 사번, 이름 순으로 조회
select employee_id ,first_name from employees where first_name like'H_ _ _ _ _%';

문제) 사원테이블에서 이름에 s자가 포함되어 있지 않은 사원들만 사번, 이름으로 검색하시오
select employee_id, first_name from employees where first_name not like'%s%';

```
### ORDER BY (정렬)
- 질의 결과에 반환되는 행들을 특정 기준으로 정렬하고자 할 때 사용
- ORDER BY절은 SELECT절의 가장 마지막에 기술
- ASC : 오름차순(생략가능)
- DESC: 내림차순(생략불가)

```
예) 사원테이블에서 급여를 많이받는 사원순으로 사번, 이름, 급여, 입사일을 출력하시오 단, 급여가 같을 경우
     입사일이 빠른순으로 정렬
SELECT EMPLOYEE_ID,FIRST_NAME,SALARY,HIRE_DATE FROM EMPLOYEES ORDER BY SALAY DESC, hire_date ASC;
오름차순은 안써도 defalut로 적용 조건(WHERE)절이 없다면 그냥 FROM 뒤에 ORDER BY를 적는다.

정렬을 하고 봤더니 똑같은 급여를 받는 사람이 생각보다 많다. 이런 경우에 먼저 입사한 사람을 먼저 보여주면 좋겠는데 내가 정렬
하고자 하는 기준이 똑같을 때 다른 정렬 기준을 두번째 세번째 추가적으로 정렬기준을 정할 수 있다.

문제)사원테이블에서 부서번호가 빠른순, 부서번호가 같다면 직종이 빠른순, 직종까지 같다면 급여를 많이 받는순으로
	사번,이름, 부서번호, 직종,급여순으로 출력

select employee_id, first_name, department_id, job_id, salary
from employees 
order by department_id, job_id, salary DESC;

문) 급여가 15000이상인 사원들의 사번, 이름, 급여, 입사일을 입사일이 빠른 순으로 조회

SELECT EMPLOYEE_ID, FIRST_NAME, SALARY, HIRE_DATE FROM EMPLOYEES WHERE SALARY >=15000 ORDER BY HIRE_DATE ASC;
```

