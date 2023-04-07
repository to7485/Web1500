## DML(Data Manipulation Language) : 데이터 조작어
 
 -SELECT:조회(검색)
   -SELECT 컬럼명1,컬럼명2... FROM 테이블명
   -WHERE 조건식;

-INSERT:추가
   -INSERT INTO 테이블명(컬럼명1, 컬럼명2...) VALUES(값1,값2...) DEFAULT 쓸 때
   -INSERT INTO 테이블명 VALUES(값1,값2,....) 무조건 만든 컬럼 개수만큼 값을 넣어야함

-UPDATE:수정
   UPDATE 테이블명<br>
   SET 기존컬럼명 = 새로운 값<br>
   WHERE 조건식 조건을 달지 않으면 테이블 전체가 바뀌어 버린다.<br>

-DELETE:삭제	TRUNCATE는 다 날려버리는건데 DELETE는 1개씩 지운다.
   DELETE FROM 테이블명	행 1개가 통째로 날아감<br>
   WHERE 조건식<br>

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
```
SELECT * FROM FLOWER; *는 컬럼명부터 검색을 하기 때문에 실제로 컬럼을 쳐주는게 속도가 훨씬 빠르다. 지금은 데이터가 별로 없어서 차이가 안나지만 나중에 백만, 천만개 있으면 속도 차이가 분명히 난다.

SELECT FLOWERNAME,FLOWERCOLOR, FLOWERPRICE FROM FLOWER;

INSERT INTO FLOWER
(FLOWERNAME, FLOWERCOLOR, FLOWERPRICE)
VALUES('장미','RED',3000);

INSERT INTO FLOWER -> 영역 드래그 하고 CTRL+ALT + ↓ 누르면 복사됨
(FLOWERNAME, FLOWERCOLOR, FLOWERPRICE)
VALUES('장미','RED',3000); -> 똑같은 값을 넣으면 오류남

INSERT INTO FLOWER
(FLOWERNAME, FLOWERCOLOR, FLOWERPRICE)
VALUES('해바라기','YELLOW',5000);

실제로 고객이 값을 넣고 컨트롤 엔터를 치지않는다. 버튼을 클릭하거나 바코드를 대거나 등록하기 버튼을 누른다거나...이런것들을 필요로 하면은 사실 데이터베이스 하나만으로는 뭔갈 만들기가 힘들다. 데이터베이스는 하나의 도구일 뿐이고 입력받거나 하는건 어플리케이션 ,모바일,웹 이런걸 필요로 한다.

INSERT INTO FLOWER
(FLOWERNAME, FLOWERCOLOR, FLOWERPRICE)
VALUES('할미꽃','WHITE',9000);

-- 추가 : 부모와 자식 중 부모테이블의 값을 먼저 추가해야 한다.
부모에 값이 없는데 어떻게 외래키로 가져다 쓸꺼냐

INSERT INTO POT
(POTID, POTCOLOR, POTSHAPE, NAME)
VALUES('20210505001','WHITE','물레방아','장미');

INSERT INTO POT
(POTID, POTCOLOR, POTSHAPE, NAME)
VALUES('20210505002','BLACK','타원형','해바라기');

INSERT INTO POT
(POTID, POTCOLOR, POTSHAPE, NAME)
VALUES('20210505003','RED','사각형','할미꽃');

INSERT INTO POT
(POTID, POTCOLOR, POTSHAPE, NAME)
VALUES('20210505004','RED','타원형','할미꽃');

--삭제 : 부모와 자식중 자식테이블에서 참조하는 값들을 먼저 삭제해야 한다.

DELETE FROM POT WHERE NAME = '장미';
DELETE FROM FLOWER WHERE FLOWERNAME = '장미';
```


## SQL함수
- 사용자가 필요한 기능을 만드는 함수가 아닌, 오라클 자체적으로 제공하는 함수
- 상황에 맞는 적절한 함수를 사용하기 위해서는 어떤 기능을 하는 함수들이 존재하는지 정확하게 파악하고 있어야 한다.

### 내장함수의 종류
- 단일행 함수 : 1개의 행값이 함수에 적용되어 1개의 행을 반환한다.
- 그룹 함수 : 1개 이상의 행의 값이 함수에 적용되어 1개의 값을 반환한다.

## 문자함수
|함수|기능|
|---|------|
|ASCII|지정된 문자의 ASCII값을 반환한다.|
|CHR|지정된 수치와 일치하는 ASCII코드를 반환한다.|
|RPAD|왼쪽 정렬 후 오른쪽에 생긴 빈공백에 특정 문자를 채워 반환한다.|
|LPAD|오른쪽 정렬 후 오른쪽에 생긴 빈공백에 특정 문자를 채워 반환한다.|
|TRIM|문자열 공백 문자들을 삭제한다.|
|RTRIM|문자열 오른쪽(뒤)의 공백 문자들을 삭제한다.|
|LTRIM|문자열 왼쪽(뒤)의 공백 문자들을 삭제한다.|
|LOWER|지정된 문자를 소문자로 반환한다.|
|UPPER|지정된 문자를 대문자로 반환한다.|
|INSTR|특정 문자의 위치를 반환한다.|
|INITCAP|지정된 문자열의 첫 단어를 대문자로 나머지는 소문자로 반환한다.|
|SUBSTR|시작 위치부터 선택 개수만큼 문자를 반환한다.|
|LENGTH|문자열의 길이를 반환한다.|
|REPLACE|첫 번째 파라미터로 지정한 문자를 두번째 파라미터로 지정한 문자로 바꿔준다.|

```
-- 지정된 문자 ASCII값을 반환한다.
SELECT ASCII('A') FROM DUAL; --결과 : 65

-- 지정된 수치와 일치하는 ASCII코드를 반환한다.
SELECT CHR(65) FROM DUAL; --결과 A

-- 왼쪽 정렬 후 오른쪽에 생긴 빈 공백에 특정 문자를 채워 반환한다.
-- RPAD(데이터,고정길이,문자)
-- 고정길이 안에 데이터를 출력하고 남는 공간을 문자로 채운다.

SELECT RPAD(DEPT_NAME,10,'*') FROM DEPT;

-- 오른쪽 정렬 후 왼쪽에 생긴 빈 공백에 특정 문자를 채워 반환한다.
-- LPAD(데이터,고정길이,문자)
-- 고정길이 안에 데이터를 출력하고 남는 공간을 문자로 채운다.

SELECT LPAD(DEPT_NAME,10,'*') FROM DEPT;

-- 문자열 공백 문자들을 삭제한다
SELECT TRIM('  HELLOW  ') FROM DUAL;

-- 컬럼이나 대상 문자열에서 특정 문자가 첫 번째나 마지막 위치에 있으면, 해당 특정 문자를 잘라낸 후 남은 문자열만 반환한다.
SELECT TRIM('zzz' FROM 'zzzHELLOWzzz') FROM DUAL;

-- 문자열 오른쪽(뒤)의 공백 문자들을 제거한다.
SELECT RTRIM('HELLOW  ')FROM DUAL;

-- 문자열 왼쪽(앞)의 공백 문자들을 제거한다.
SELECT LTRIM('   HELLOW')FROM DUAL;

-- 특정문자의 위치를 반환한다.
SELECT INSTR('HELLOW','O') FROM DUAL;

- 문자열에서 1번째 자리부터 검색하여 두번째로 나오게 되는 글자가 위치하는 자리를 반환한다.
SELECT INSTR('HELLOW','L',1,2) FROM DUAL;

-- 찾는 문자열이 없으면 0을 반환한다.
SELECT INSTR('HELLOW','Z') FROM DUAL;

--첫 문자를 대문자로 변환하는 함수 공백이나 /를 구분자로 인식함
select initcap('good morning') from dual;
select initcap('good/morning') from dual;

-- 문자열의 길이를 반환한다.
select length('john') from dual;

-- 첫 번째 지정한 문자를 두번째 지정한 문자로 바꿔 반환한다.

문) 부서번호가 50번인 사원들의 이름을 출력하되 이름중 'el'을 모두 '**'로 대체하여 출력하시오
SELECT REPLACE(FIRST_NAME,'el','**') FROM EMPLOYEES WHERE DEPARTMENT_ID = 50;

```

## 숫자함수
|함수|기능|
|---|------|
|ABS|절대값을 반환한다.|
|ROUND|특정 자리수에서 반올림을 하여 반환한다.|
|FLOOR|주어진 숫자보다 작거나 같은 정수중 최대값을 반환한다.|
|TRUNC|특정 자리수에서 잘라내고 반환한다.|
|SIGN|주어진 값의 음수,정수,0 여부를 반환한다.|
|CEIL|주어진 숫자보다 크거나 같은 정수 중 최소값을 반환한다.|
|MOD|나누기 후 나머지를 반환한다.|
|POWER|주어진 숫자의 지정된 수 만큼의 제곱값을 반환한다.|

```
-- 절대값을 반환한다.
SELECT -10,ABS(-10) FROM DUAL;

-- 특정 자리수에서 반올림하여 반환한다.
-- 지정한 숫자가 양수이면 소수점 아래, 음수이면 소수점 위를 의미한다. 생략되면 반올림해서 정수를 반환한다
SELECT ROUND(1234.567,1),ROUND(1234.567,-1),ROUND(1234.567) FROM DUAL;

-- 주어진 숫자보다 작거나 같은 정수 중 최대값을 반환한다.
SELECT FLOOR(2), FLOOR(2.1) FROM DUAL;

-- 특정 자리수를 버리고 반환한다.
SELECT TRUNC(1234.567,1),TRUNC(1234.567,-1),TRUNC(1234.567) FROM DUAL;

-- 주어진 값의 음수,정수,0 여부를 반환한다.
--음수는 -1, 0은 0, 양수는 1, NULL은 NULL을 반환한다.
SELECT SIGN(-10),SIGN(0),SIGN(10),SIGN(NULL) FROM DUAL;

-- 주어진 숫자보다 크거나 같은 정수 중 최소값을 반환한다.
SELECT CEIL(2),CEIL(2.1) FROM DUAL;

-- 나누기 후 나머지를 반환한다.
SELECT MOD(1,3),MOD(2,3),MOD(3,3),MOD(4,3),MOD(0,3) FROM DUAL;

-- 주어진 숫자의 지정된 수 만큼 제곱값을 반환한다.
SELECT POWER(2,1),POWER(2,2),POWER(2,3),POWER(2,0) FROM DUAL;

```

## 날짜함수
|함수|기능|
|---|------|
|ADD_MONTHS|특정날짜에 개월수를 더해준다. |
|MONTHS_BETWEEN|주어진 두 개의 날짜 간격 개월을 반환한다.|
|NEXT_DAY|주어진 일자가 다음에 나타나는 지정요일(1:일요일 ~ 7:토요일)의 날짜를 반환한다.|
|LAST_DAY|주어진 일자가 포함된 월의 말일을 반환한다.|

※ 날짜 + 날짜 : 날짜끼리는 더하기가 안됩니다.

- SYSDATE : 현재 날짜

```
-- 특정 날짜에 개월수를 더한다.

select sysdate, add_months(sysdate, 2)from dual;

문제) 사원테이블에서 모든 사원의 입사일로부터 6개월 뒤의 날짜를 이름, 입사일, 6개월뒤 날짜 순으로 출력
select first_name, hire_date,add_months(hire_date,6) new_date from employees;

문제) 사번이 120번인 사원이 입사후 3년 6개월째 되는날 진급예정이다. 진급 예정 날짜를 구하시오.
select fist_name, add_months(hire_date, 42) promotion from employees where employee_id = 120;

select trunc( months_between(sysdate, '01/01/2015'), 2)from dual;

2) MONTHS_BETWEEN : 두 날짜 사이의 개월수를 구한다

문제)모든 사원들이 입사일로부터 오늘가지 몇개월이 경과했는지 출력

SELECT sysdate, hire_date, MONTHS_BETWEEN(sysdate,hire_date) FROM EMPLOYEES;

문제) 사원들의 이름, 입사일, 입사후 오늘까지의 개월수를 조회하되 입사기간이 200개월 이상인 사람만 출력하고 입사개월수는
소수점 이하 한자리까지만 버림하시오.

SELECT FIRST_NAME, HIRE_DATD, TRUNC(MONTHS_BETWEEN(SYSDATDE,HIRE_DATE),1) MONTHS FROM EMPLOYEES
WHERE TRUNC(MONTHS_BETWEEN(SYSDATDE,HIRE_DATE),1) >= 200;

문제)입사기간이 160개월 이상인 사원들의 이름, 입사일, 입사후 개월수를 출력

select first_name, hire_date,mon from employees where trunc(months_between(sysdate, hire_date)>=200),0) mon

문제) 사번이 120번인 사원이 입사후 3년 6개월이 되는날 퇴사했다. 이 사원의 사번,이름,입사일,퇴사일을 출력

SELECT EMPLOYEE_ID, FIRST_NAME, HIRE_DATE, add_months(HIRE_DATE, 42) fired FROM EMPLOYEES WHERE EMPLOYEE_ID = 120;
```

오라클에서는 문자열을 날짜형 데이터로 형 변환을 하기 위해서 TO_DATE()함수를 사용합니다.
- TO_DATE('문자열','날짜포맷')

```
SELECT to_char(sysdate,'yyyy-mm-dd') FROM dual;
SELECT to_char(sysdate,'yyyy-mm-dd day') FROM dual;
SELECT to_char(sysdate,'yyyy-mm-dd HH:MI:SS') FROM dual;
```

### 날짜형식 FORMATTING 모델
|모델|기능|
|---|------|
|SCC, CC|세기|
|YYYY,YY|연도|
|MM|월|
|DD|일|
|DAY|요일|
|MON|월명(JAN)|
|MONTH|월이름이 다나온다(JANUARY)|
|HH,HH24|시간|
|MI|분|
|SS|초|

<hr>

## 그룹함수
- 여러 행들에 대한 연산의 결과를 하나의 행으로 반환한다.

|함수|기능|
|---|------|
|COUNT|행들의 개수를 반환|
|MIN|행들의 최소값을 반환|
|MAX|행들의 최대값을 반환|
|SUM|행들의 합계를 반환|
|AVG|행들의 평균을 반환한다.|
|STDDEV|행들의 표준편차를 반환한다.|
|VARIANCE|행들의 분산을 반환한다.|

```
**일반적으로 그룹함수와 일반컬럼을 함께 쓸 수 없다.
select count(*), first_name from employees;

예) 사원테이블의 전체 사원수를 출력
select COUNT(*) FROM EMPLOYEES;

예) 사원테이블에서 보너스를 받는 사원의 수를 출력
select count(commIssion_pct) from employees;

문제) 사원테이블에서 직종이 SA_REP인 사원들의 평균급여, 급여최고액, 급여최저액, 급여의 총 합계를 출력하시오.
select AVG(salary) AVG,MAX(salary) MAX,MIN(salary) MIN,SUM(salary) SUM
from employees
WHERE JOB_ID = 'SA_REP';

예) 부서의 갯수를 출력
select count(DISTINCT department_id)from employees;

DISTINCT : 중복된 값은 세지 않는다.

문) 부서번호가 80번인 부서의 사원들의 급여의 평균을 출력

SELECT ROUND(AVG(SALARY), 1) AVG FROM EMPLOYEES WHERE DEPARTMENT_ID = 80;

예) 관리자의 수를 출력
distinct : 중복된 값을 제외
select count(distinct manager_id) from employees;


문제) 사원 테이블에 등록되어있는 모든 사원의 수, 보너스를 받는 인원수, 전체사원 급여의 평균, 등록되어있는 부서의 갯수를 화면에 출력
select count(employee_id),count(
				select count(employee_id),count(commission_pct),avg(salary),count(distinct department_id) 
				from employees; commission_pct),avg(salary),count(distinct department_id) 
				from employees;

문제) 사원테이블에서 80번 부서에 속하는 사원들의 연봉의 평균을 소수점 두자리까지 반올림하여 출력하시오
select round(AVG(salary), 2) from employees where department_id = 80;

문제) 사원테이블에서 50번에 속하는 사원들의 급여의 최대값과 최소값을 출력하세요
select Max(salary),MIN(SALARY), FROM EMPLOYEES WHERE DEAPRTMENT_ID = 50;
```
## GROUP BY(그룹화)
- 특정테이블에서 소그룹을 만들어 결과를 분산시켜 얻고자 할 때
- GROUP BY : ~별 (예 : 부서별 인원수)
```
예) 각 부서별 급여의 평균과 총 합을 출력

SELECT DEPARTMENT_ID, COUNT(*), AVG(SALARY), SUM(SALARY) FROM EMPLOYEES GROUP BY DEPARTMENT_ID;

GROUP BY로 소그룹을 지정하고 싶은 컬럼이 있는데 이걸 SELECT에서도 사용하고 싶으면 GROUP BY에 작성해놓은 컬럼명만 SELECT에 추가할 수 있다.

예) 부서별, 직종별로 그룹을 나눠서 인원수를 출력 단, 부서번호가 낮은순으로 정렬하시오.

SELECT DEPARTMENT_ID, JOB_ID, COUNT(*) FROM EMPLOYEES GROUP BY DEPARTMENT_ID, JOB_ID ORDER BY DEPARTMENT_ID;

부서가 같아도 직종이 다른 경우가 있기 때문에 좀더 세부적인 결과물을 가져올수 있는게 GROUP BY다.

문) 각 직종별 인원수를 출력

SELECT JOB_ID, COUNT(*) FROM EMPLOYEES GROUP BY JOB_ID;

문) 각 직종별 급여의 합을 출력

SELECT JOB_ID, SUM(SALARY) FROM EMPLOYEES GROUP BY JOB_ID;
괄호안에 어떤걸 넣어야 하는지 알면 난이도가 많이 내려간다.

문) 부서별로 가장 높은 급여를 조회

SELECT DEPARTMENT_ID, MAX(SALARY) FROM EMPLOYEES GROUP BY DEPARTMENT_ID;

문) 부서별 급여의 합계를 내림차순으로 조회

SELECT DEPARTMENT_ID, SUM(SALARY) FROM EMPLOYEES GROUP BY DEPARTMENT_ID ORDER BY SUM(SALARY) DESC;
```









