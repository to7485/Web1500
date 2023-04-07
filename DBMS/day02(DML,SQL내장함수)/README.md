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









