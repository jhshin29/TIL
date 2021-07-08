#SQL 복습문제 1

## SQL select 문을 사용하여 데이터 검색

```sql
/*
emp 테이블의 컬럼은 다음과 같습니다.
empno : 사원번호
ename : 사원이름
sal : 월급
job : 직무
mgr : 관리자의 사원번호
hiredate : 입사일
comm : 커미션
deptno : 부서번호
*/

--문제1-1. dept 테이블의 모든 컬럼을 검색하시오 !
select *
from dept;
--문제1-2. 사원번호,이름, 월급을 출력하시오 !
select empno, ename, sal
from emp;
--문제1-3. 이름, 직무, 입사일, 부서번호를 출력하시오 !
select ename, job, hiredate, deptno
from emp;
--문제1-4. 이름, 월급, 커미션을 출력하시오 !
select ename, sal, comm
from emp;
--문제1-5. 이름, 월급, 커미션, 월급 + 커미션을 출력하시오!(comm이 null인 경우는 1로 대체합니다.)
select ename, sal, comm, sal+nvl(comm,1) as "sal+comm"
from emp;
--문제1-6. 직무를 출력하는데 중복을 제거해서 출력하시오 !
select distinct job
from emp;
--문제1-7. 부서번호를 출력하는데 중복제거해서 출력하시오
select distinct deptno
from emp;
문제1-8. 이름와 월급을 연결해서 출력하시오 !
select ename || sal
from emp;

select concat(ename, sal)
from emp;

/*
문제1-9. 아래와 같이 결과가 출력되게하시오 !
EMPLOYEE
----------------------------------------------------------
SMITH's job is CLERK
ALLEN's job is SALESMAN
...
14 rows selected.
*/
select ename||'''s job is '||job --문자열에 ' 쓰려면 2번 쓰면 됨
from emp;
```

## 데이터 제한 및 정렬

```sql
--문제2-1. 월급이 3000 인 사원들의 이름과 월급을 출력하시오 !
select ename, sal
from emp
where sal = 3000;
--문제2-2. 직무가 SALESMAN 인 사원들의 이름과 직무를 출력하시오!
select ename, job
from emp
where job = 'SALESMAN';
--문제2-3. 월급이 2500 이상인 사원들의 이름과 월급을 출력하시오
select ename, sal
from emp
where sal >= 2500;
--문제2-4. 직무가 SALESMAN 이 아닌 사원들의 이름과 직무를 출력하시오 !
select ename, job
from emp
where job != 'SALESMAN';
--문제2-5. 이름, 연봉을 출력하시오. 연봉은 sal * 12 로 출력하고 컬럼명을 Sum_Sal로 출력하시오!
select ename, (sal*12) "Sum_Sal"
from emp;
--문제2-6. 연봉이 36000 이상인 사원들의 이름과 연봉을 출력하시오!
select ename, sal*12
from emp
where sal*12 >= 36000;
--문제2-7. 월급이 1000 에서 3000 사이인 사원들의 이름과 월급을 출력하시오 !
select ename, sal
from emp
where sal between 1000 and 3000;
--문제2-8. 월급이 1000 에서 3000 사이가 아닌 사원들의 이름과 월급을 출력하시오 !
select ename, sal
from emp
where sal not between 1000 and 3000;
--문제2-9. 80년 12월 17일에 입사한 사원의 이름과 입사일을 출력하시오!
select ename, hiredate
from emp
where hiredate = to_date('80/12/17', 'RRRR/MM/DD');
--문제2-10. 1981 년도에 입사한 사원들의 이름과 입사일을 출력하시오
select ename, hiredate
from emp
where hiredate between to_date('1981/01/01', 'RRRR/MM/DD') 
                and to_date('1981/12/31', 'RRRR/MM/DD') - 1/86400;
--문제2-11. 이름의 첫글자가 S 로 시작하는 사원들의 이름을 출력하시오 !
select ename
from emp
where ename like 'S%';
--문제2-12. 이름의 끝글자가 T 로 끝나는 사원들의 이름을 출력 하시오 !
select ename
from emp
where ename like '%T';
--문제2-13. 이름의 두번째 철자가 M 인 사원들의 이름을 출력하시오!
select ename
from emp
where ename like '_M%';
--문제2-14. 이름에 세번째 철자가 L 인 사원들의 이름을 출력하시오!
select ename
from emp
where ename like '__L%';
--문제2-15. 문제를 위해서 data 를 한건 입력한다.
--insert into emp(empno,ename,sal)
--values( 1234,'A%B', 3400) ;
--이름의 두번째 철자가 % 인 사원의 이름을 출력하시오!
select ename
from emp
where ename like '_v%%' escape 'v';
--문제2-16. 문제를 위해서 아래의 데이터를 입력하시오 !
--insert into emp(empno,ename,sal)
--values( 2919,'A%%B', 4300 );
--이름의 두번째 철자와 세번째 철자가 % 인 사원의 이름을 출력하시오 !
select ename
from emp
where ename like '_v%v%%' escape 'v';
--문제2-17. 이름의 첫번째 철자가 S 가 아닌 사원들의 이름을 출력하시오 !
select ename
from emp
where ename not like 'S%';
--문제2-18. 사원번호가 7788, 7902, 7369번인 사원들의 사원번호와 이름을 출력하시오 !
select empno, ename
from emp
where empno in (7788, 7902, 7369);
--문제2-19. 직무가 SALESMAN, ANALYST 가 아닌 사원들의 이름과 직무를 출력하시오 !
select ename, job
from emp
where job not in ('SALESMAN', 'ANALYST');
--문제2-20. 커미션이 null 인 사원들의 이름과 커미션을 출력하시오!
select ename, comm
from emp
where comm is null;
--문제2-21. 커미션이 null 이 아닌 사원들의 이름과 커미션을 출력하시오 !
select ename, comm
from emp
where comm is not null;
--문제2-22. 이름과 월급을 출력하는데 월급이 낮은 사원부터 출력되게하시오 !
select ename, sal
from emp
order by sal;
--문제2-23. 이름과 입사일을 출력하는데 최근에 입사한 사원부터 출력하시오 !
select ename, hiredate
from emp
order by hiredate desc;
--문제2-24. 직무가 SALESMAN 인 사원들의 이름, 월급을 출력하는데 월급이 많은 사원부터 출력하시오 !
select ename, sal
from emp
where job = 'SALESMAN'
order by sal desc;
--문제2-25. 부서번호가 10, 20 번이 아닌 사원들의 이름과 입사일을 출력하는데 먼저 입사한순서로 출력 되게 하시오 !
select ename, hiredate
from emp
where deptno not in (10,20)
order by hiredate;
--문제2-26. 커미션이 null 이 아닌 사원들의 이름과 월급을 출력하는데 월급이 높은 사원부터출력하시오 !
select ename, sal
from emp
where comm is not null
order by sal desc;
--문제2-27. 직무가 SALESMAN 이고 월급이 1000 이상인 사원들의 이름과 월급과 직무를 출력하시오 !
select ename, sal, job
from emp
where job = 'SALESMAN' and sal >= 1000;
--문제2-28. 이름의 첫글자가 S 로 시작하거나 월급이 1000 이상인 사원들의 이름과 월급을 출력하시오 !
select ename, sal
from emp
where ename like 'S%' and sal >= 1000;
--문제2-29. 이름, 직무, 입사일을 출력하는데 직무를 ABCD 순으로 정렬한 상태에서 입사일을
--각각 최근에 입사한 사원부터 출력되게하시오 !
select ename, job, hiredate
from emp
order by job , hiredate desc;
--문제2-30. 81년 4월 2일에 입사한 사원의 이름과 입사일을 출력하시오 !
select ename, hiredate
from emp
where hiredate = '1981/04/02';
--문제2-31. 81년도에 입사한 사원들의 이름과 입사일을 출력하시오 !
select ename, hiredate
from emp
where hiredate between '1981/01/01' and '1981/12/31';
--문제2-32. 위의 결과를 like 연산자로 수행해보시오 !
select ename, hiredate
from emp
where hiredate like '1981%';
```