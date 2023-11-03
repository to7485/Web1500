# SUBQUERY
- 특정 sql문장 안에 또 다른 sql문장이 포함되어 있는 것
- 여러번 DB접속이  필요한 상황을 한번으로 줄여서 속도를 증가시킬 수 있다.
- 서브쿼리를 사용할 수 있는 곳.
  1) where, having
  2) select, delete문의 from절
  3) update문의 set
  4) insert문의 into
```SQL
예) 사원테이블에서 이름이 'Michael'이고, 직종이 'MK_MAN'인 사원의 급여보다
많이 받는 사원들의 정보를 사번, 이름, 직종, 급여 순으로 출력

데이터베이스에 한번만 접근을 하는걸로는 답을 구하기가 어렵다.

1) 이름이 Michael이고, 직종이 'MK_MAN'인 사원의 급여 구하기
  select salary from employees where first_name = 'Michael' and job_id = 'MK_MAN'; --13000

2)13000보다 급여가 높은 사원들의 정보
  select employee_id, first_name, job_id, salary from employees where salary > 13000;

SELECT문을 몇개 작성해야하는지 알면 SUBQUERY를 작성하는건 어렵지 않다.

3) 위의 두 쿼리문을 subquery를 통해 하나로 합쳐보자
select employee_id, first_name, job_id, salary
from employees
where salary > (select salary
		 from employees 
		 where first_name = 'Michael' and job_id = 'MK_MAN');

문제) 사번이 150번인 사원의 급여와 같은 급여를 받는 사원들의 정보를 사번, 이름, 급여 순으로 출력
select employee_id, first_name, salary
from employees 
where salary = (select salary 
		from employees 
		where employee_id = 150);

문제) 월급이 회사의 평균월급 이상인 사람들의 이름과 월급을 조회
select first_name, salary from employees where salary>=(select avg(salary) from employees);


문제) 사번이 111번인 사원의 직종과 같고 사번이 159번인 사원의 급여보다 많이 받는 사원들의 정보를 사번, 이름, 직종, 급여 순으로 출력
select employee_id,FIRST_NAME, job_id, salary 
from employees 
where JOB_ID = (SELECT JOB_ID FROM EMPLOYEES
		WHERE EMPLOYEE_ID = 111)
		AND SALARY > (SELECT SALARY 
				FROM EMPLOYEES
				WHERE EMPLOYEE_ID = 159);

문제) 직종, 평균급여를 출력하되
평균급여가 Bruce사원보다 큰(초과) 경우를 조회
SELECT JOB_ID, AVG(SALARY)
FROM EMPLOYEES
GROUP BY JOB_ID,
HAVING AVG(SALARY) > (SELECT SALARY
			FROM EMPLOYEES
			WHERE FIRST_NAME = 'Bruce');

문제) 사원테이블에서 성에 'Bat'이라는 단어를 포함하고 있는 사원과 같은 부서에서 근무하는 사원의 부서번호,이름을 출력
select department_id, first_name, last_name
from employees
where department_id = (select department_id 
			from employees 
			where last_name like 'Bat%');
      
문제) Bruce와 같은 부서에서 근무하고 있는 사원들의 이름을 출력
select first_name from employees where department_id = (select department_id from employees where first_name='Bruce');


문제) 사워테이블에서 last_name이 Kochhar의 급여보다 많이받는 사원들의 사번, 성,직종, 급여를 출력
select employee_id, last_name, job_id,salary from employees where salay > (select salary from employees where last_name ='Kochhar');

문제) 사원테이블에서 급여가 가장 적은 사람의 사번, 이름, 급여를 출력
select employee_id, first_name, salary from employees where salary = (select MIN(salary) from employees);

문제) 사원테이블에서 직종이 SA_REP인 사원의 최소급여보다 적으면서(미만) 직종이 SH_CLERK은 아닌 사원들의 이름 직종 급여를 출력
select first_name, job_id, salary from employees where salary <(select MIN(salary) from employees where job_id = 'SA_REP') and not job_id = 'SH_CLERK';
														-- job_id !='SH_CLERK'
														-- job_id NOT IN('SH_CLERK')
														-- job_id <> 'SH_CLERK';

문제) 사원테이블에서 100번 부서의 최소 급여보다 많이 받는(초과) 다른 모든 부서의 부서번호, 최소급여를 출력하세요
select department_id,MIN(salary) from employees group by department_id having MIN(salary) > (select MIN(salary) from employees where department_id = '100');


문제) 137번 사원보다 월급이 크거나 같고 149번 사원보다는 월급이 작거나 같은 사원의 이름과 월급을 조회
select first_name, salary 
from employees 
where salary between (select salary 
                      from employees where employee_id = 137) and (select salary 
                                                                   from employees where employee_id = 149);
```

# JOIN
- 데이터를 불러올 때 하나의 테이블만 사용하는 것이 아니라 두 개 이상의 테이블을 합쳐서 필요한 데이터를 추출할 때 사용하는 것이 조인기능이다.
- JOIN을 사용하려면 두개의 테이블이 적어도 하나의 컬럼을 공유해야 한다.
- 보통 기본키(PK)와 외래키(FK)값을 이용하여 JOIN을 한다.

## JOIN의 종류
### 1. Inner Join
- 각 테이블에서 조인 조건에 일치되는 데이터만 가져온다.
- A와 B테이블의 공통된 부분을 의미한다 보통 교집합 이라고 부른다.

```SQL
테이블명A INNER JOIN 테이블명B ON 조건식
테이블명A JOIN 테이블명B ON 조건식
```

![image](https://user-images.githubusercontent.com/54658614/230700010-b6b1bb7d-ad38-4478-bff7-d029b76c05b1.png)

- INNER JOIN은 교집합 연산과 같다. JOIN 하는 컬럼 값이 양쪽 테이블 데이터 집합에서 공통적으로 존재하는
데이터만 조인해서 결과 데이터 집합으로 추출

```
<사용법>
SELECT 컬럼명1,컬럼명
FROM TABLE A (INNER) JOIN TABLE B
ON TABLEA.조인컬럼 = TABLEB.조인컬럼

SELECT 컬럼명1,컬럼명
FROM TABLE A , TABLE B
WHERE TABLEA.조인컬럼 = TABLEB.조인컬럼

```
2. LEFT OUTER JOIN
- 교집합의 연산결과와 차집합의 연산 결과를 합친것과 같다.
- LEFT OUTER JOIN 왼쪽에 테이블에만 존재하는 데이터만 결과로 추출한다.

![image](https://user-images.githubusercontent.com/54658614/230700152-a35a75ee-fa7a-4560-8f46-06075f033af4.png)

```
<사용법>

SELECT 컬럼명1,컬럼명
FROM TABLE A LEFT OUTER JOIN TABLE B
ON TABLEA.조인컬럼 = TABLEB.조인컬럼

```
3. RIGHT OUTER JOIN
- LEFT OUTER JOIN의 반대이다.
- RIGHT OUTER JOIN 키워드의 오른쪽에 명시된 테이블에만 존재하는 데이터를 추출한다.

![image](https://user-images.githubusercontent.com/54658614/230700239-6fd82096-045a-4fbc-a077-25fbd4d78201.png)

<사용법>

```
SELECT 컬럼명1,컬럼명
FROM TABLE A RIGHT OUTER JOIN TABLE B
ON TABLEA.조인컬럼 = TABLEB.조인컬럼
```
4. FULL OUTER JOIN
- 합집합 연산 결과와 같다.
- 양쪽 테이블 데이터 집합에서 공통적으로 존재하는데이터, 한쪽에만 존재하는 데이터 모두 추출한다.

![image](https://user-images.githubusercontent.com/54658614/230700279-881f5fff-cc77-4f2d-8cfe-1c166f614356.png)

```
<사용법>

SELECT 컬럼명1,컬럼명
FROM TABLE A FULL OUTER JOIN TABLE B
ON TABLEA.조인컬럼 = TABLEB.조인컬럼

```

```
SELECT e.first_name, e.department_id, d.department_name -- -> 조건을 안써주면 그냥 맨위에꺼 가져옴
from employees e JOIN DEPARTMENTS d
ON e.department_id = d.department_id; ---> join에는 조건이 없을 수 없다.

--예) 부서(department), 지역(Locations)로 부터 부서이름과 city를 출력해보자

SELECT d.department_name, l.city 
FROM DEPARTMENTS d JOIN locations l
ON d.location_id = l.location_id;

--문) 지역(Locations), 나라(countries)를 조회하여 도시명과 국가명을 조회

SELECT L.CITY,C.COUNTRY_NAME
FROM LOCATIONS L JOIN COUNTRIES C
ON L.COUNTRY_ID = C.COUNTRY_ID;

--문) FIRST_NAME, LAST_NAME, JOB_ID, JOB_TITLE을 출력(EMPLOYEES,JOBS 테이블을 조회)

SELECT E.FIRST_NAME, E.LAST_NAME, J.JOB_ID, J.JOB_TITLE
FROM EMPLOYEES E JOIN JOBS J
ON E.JOB_ID = J.JOB_ID;

--문) 사원, 부서, 지역테이블로부터 FIRST_NAME, EMAIL, DEPARTMENT_ID, DEPARTMENT_NAME, LOCATION_ID,CITY
--정보를 출력하되, CITY가 'Seattle'인 경우만 출력하시오.

select e.first_name, e.email, e.department_id, d.department_name, d.location_id, l.city
from employees e JOIN departments d ON e.department_id = d.department_id
JOIN locations l ON d.location_id = l.location_id
and l.city = 'Seattle';
```
## VIEW

기존의 테이블은 그대로 놔둔 채<br>
필요한 컬럼들 및 새로운 컬럼을 만든 가상의 테이블.<br>
실제 데이터가 저장되는 것은 아니지만<br>
view를 통해서 데이터를 관리할 수 있다.<br>

### view의 특징

- 독립성 : 다른곳에서 변경하지 못하도록

- 편리성 : 긴 쿼리문을 짧게
      - 내가 원하는 조건을 쳐서 가져와야 되는데 조건이 만들어진 view에서 접근을 하면 서브쿼리를 쓸 필요가 없어진다.

- 보안성 : 짧게 만들기 때문에 기존의 쿼리는 보이지 않는다.

```
CREATE VIEW 뷰이름 AS
(
  쿼리문
)
```

```
create view MY_EMPL AS
(
select employee_id, first_name, salary
from employees
);
SELECT * FROM MY_EMPL;

------------------------------------------------------------

create view MY_EMPL AS---> AS가 없으면 VIEW가 만들어 지지 않는다.
(
select employee_id, first_name, salary, (SALARY * COMMISSION_PCT) COMM
FROM EMPLOYEES
);
SELECT * FROM MY_EMPL;

---------------------------------------------------

create view DATA_PLUS AS
(
SELECT S.*,(S.COMMISSION_PCT * S.SALARY) COMM
FROM ( SELECT * FROM EMPLOYEES) S
	--EMPLOYEES S; 도 가능하다
);
SELECT * FROM DATA_PLUS;

------------------------------------------------------------

CREATE view DATA_PLUS AS
(
SELECT S.SALARY, RANK() OVER(ORDER BY SALARY DESC) rank
FROM EMPLOYEES S
);


SELECT * FROM DATA_PLUS;

------------------------------------------------------------

create view TEST AS
(
SELECT E.EMPLOYEE_ID, D.DEPARTMENT_NAME
FROM EMPLOYEES E, DEPARTMENTS D
WHERE E.DEPARTMENT_ID = D.DEPARTMENT_ID
);
SELECT * FROM TEST;
```
## SEQUENCE
- 테이블에 값을 추가할 때 자동으로 순차적인 정수값이 들어가도록 설정해주는 객체

### 시퀀스 생성하기
```
시퀀스 생성하기(create sequence 시퀀스명;)
CREATE SEQUENCE MEMO_SEQ;
Start with 1 --1부터 카운팅
INCREMENT BY 1 --1씩 증가
CACHE 20; -- 미리 20개의 INDEX공간을 확보 20명이 동시에 접속해서 글을 써도 버벅거리지 않게 해준다.
NOCACHE; -- 1개의 INDEX공간만 확보
```
### 메모테이블 만들어보기
```
create table memo(
	idx NUMBER(3) primary key,
	title VARCHAR2(50) not null,
	content VARCHAR2(4000),
	pwd VARCHAR2(20) not null,
	writer VARCHAR2(100) not null,
	IP VARCHAR2(20),
	write_date DATE
	del_info NUMBER() ----  0, -1, 1
);
```
### MEMO_SEQ를 사용하여 MEMO테이블에 값을 추가
```
insert into MEMO values(memo_seq.nextval, '제목1', '내용1', '1111', '홍길동','192.1.1',sysdate );

insert into MEMO values(memo_seq.nextval, '제목2', '내용2', '1111', '홍길동2','192.1.1',sysdate );
```
