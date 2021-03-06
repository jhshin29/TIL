## 단일행 함수

```sql
--문제3-1. 이름을 출력하는데 대문자로도 출력하고 소문자로도 출력하고 첫번째 철자는 대문자
--나머지는 소문자로도 출력하시오 !
select ename, upper(ename), lower(ename), initcap(ename)
from emp;
--문제3-2. 이름이 king 인 사원의 이름과 월급을 출력하는데 철자를 모두 소문자로 작성해서
--검색되게하시오!
select ename, sal
from emp
where lower(ename) = 'king';
--문제3-3. 이름의 첫글자가 S 로 시작하는 사원의 이름을 출력 하는데 LIKE 사용하지 말고
--substr 함수로 수행하시오!
select ename
from emp
where substr(ename,1,1) = 'S';
--문제3-4. 이름의 두번째 철자가 M 인 사원들의 이름을 출력하시오
--(Like 쓰지 말고 substr 함수로)
select ename
from emp
where substr(ename,2,1) = 'M';
--문제3-5. 이름의 끝글자가 T 로 끝나는 사원들의 이름을 출력하시오 !
select ename
from emp
where substr(ename, length(ename),1) = 'T';
--문제3-6. 이름의 시작 철자가 AL 로 시작하는 사원들의 이름을 출력하시오 !
SELECT ename
FROM emp
WHERE substr(ename,1,2) ='AL';
--문제3-7. 이름의 끝철자가 MS 로 끝나는 사원들의 이름을 출력하시오 !
select ename
from emp
where substr(ename, -2,2) = 'MS';
--문제3-8. 아래의 SQL 로 출력되는 결과를 initcap 사용하지 말고 substr, upper, lower, ||
--를 사용해서 출력되게하시오 !
--select initcap(ename)
--from emp;
--INITCAP(ENAME)
----------------------
--Smith
--Allen
--..
--14 rows selected.
select upper(substr(ename,1,1))||lower(substr(ename,2))
from emp;
--문제3-9. 이름의 첫번째 철자가 S 인 사원들의 이름을 출력 하는데 instr 을 이용해서 출력하시오!
select ename
from emp
where instr(ename, 'S') = 1;
--문제3-10. 이름의 철자의 갯수가 6개 이상인 사원들의 이름과 이름의 철자의 갯수를 출력하시오!
select ename, length(ename)
from emp
where length(ename) >= 6;
--문제3-11. 이름, 입사한 날짜부터 오늘까지 총 몇일 근무했는지 출력하시오 !
select ename, sysdate - hiredate
from emp;
--문제3-12. 이름, 입사한 날짜부터 오늘까지 총 몇달 근무했는지 출력하시오 !
SELECT ename, months_between(sysdate,hiredate)
FROM emp;
--문제3-13. 오늘부터 100 달 뒤에 날짜가 어떻게 되는지 출력하시오!
select add_months(sysdate,100)
from dual;
--문제3-14. 오늘부터 6개월 후의 날짜를 출력하시오
select add_months(sysdate,6)
from dual;
--문제3-15. 오늘부터 앞으로 돌아올 월요일의 날짜를 출력하시오!
select next_day(sysdate, '월')
from dual;
--문제3-16. 오늘부터 100 달뒤에 돌아오는 월요일의 날짜를 출력하시오 !
select next_day(add_months(sysdate,100), '월')
from dual;
--문제3-17. 이번달의 마지막 날짜를 출력하시오 !
select last_day(sysdate)
from dual;
--문제3-18. 오늘 부터 100 달 뒤에 돌아오는 날짜의 요일을 출력하시오 !
select to_char(add_months(sysdate, 100), 'DY')
from dual;
--문제3-19. 이름, 입사한 년도만 출력하시오 !
select ename, to_char(hiredate, 'YYYY')
from emp;
--문제3-20. 1981 년도에 입사한 사원들의 이름과 입사한 년도를 출력하시오 ! (to_char 를 활
--용하시오)
select ename, to_char(hiredate, 'YYYY')
from emp
where to_char(hiredate, 'YYYY') = '1981';
--문제3-21. 1981 년도에 입사한 사원들의 이름과 직무와 입사일을 출력하는데 이름과 직무를
--소문자로 출력하시오 !
select lower(ename), lower(job), to_char(hiredate, 'YYYY')
from emp
where to_char(hiredate, 'YYYY') = '1981';
--문제3-22. 81년 12월 03일에 입사한 사원의 이름과 입사일을 출력하시오 !
select ename, hiredate
from emp
where hiredate = '1981/12/03';
--문제3-23. 81년 12월 11일에 입사한 사원의 이름과 입사일을 출력하는데 where 절에 to_char
--를 이용해서 출력하시오
select ename, hiredate
from emp
where to_char(hiredate, 'YY/MM/DD') = '81/12/03';
--문제3-24. 81 년도에 입사한 사원들의 이름과 입사일을 출력하는데 nls_date_format 과 상관
--없이 무조건 결과가 출력되도록 SQL 을 작성하시오 !
select ename, to_date(hiredate)
from emp
where hiredate between '1981/01/01' and '1981/12/31';

SELECT ename, hiredate
FROM emp
WHERE hiredate
BETWEEN to_date('1981/01/01','RRRR/MM/DD')
AND to_date('1981/12/31','RRRR/MM/DD');
--문제3-25. 이름, 커미션을 출력하는데 커미션이 null 인 사원들은 no comm 이란 글씨로 출력
--되게하시오 !
select ename, nvl(to_char(comm), 'no comm' )
from emp;
--문제3-26. 이름, 입사한 년도(4자리), 월급, 보너스를 출력하는데 보너스가 입사한 년도가
--1981 년도면 5000 을 출력하고 나머지 사원들은 0 을 출력하시오 !
select ename, to_char(hiredate,'RRRR') hiredate, sal
    , decode(to_char(hiredate,'RRRR'), '1981', 5000
                                             ,0) bonus   
from emp;
--문제3-27. 이름, 월급, 직무, 보너스를 출력하는데 직무가 SALESMAN 이면 보너스를 8000 을
--출력하고 직무가 ANALYST 면 보너스를 6000 을 출력하고 직무가 CLERK 이면 보너
--스를 4000 을 출력하고 나머지 사원들은 0 을 출력하시오 !
select ename, sal, job
    , decode(job, 'SALESMAN', 8000
                , 'ANALYST', 6000
                , 'CLERK', 4000
                         , 0) bonus
from emp;
--문제3-28. 이름, 월급과 보너스를 출력하는데 월급이 3000 이상이면 보너스를 6000 을 출력하
--고 월급이 3000 보다 작으면 보너스를 0 을 출력하시오 !
select ename, sal
    , (case when sal>=3000 then 6000
            else 0 end) bonus
from emp;
--문제3-29. 이름, 커미션, 보너스를 출력하는데 보너스가 커미션이 null 이면 7000 을 출력하
--고 커미션이 null 이 아니면 0 을 출력하시오 !
select ename, comm, nvl2(comm, 0, 7000) bonus
from emp;
--문제3-30. 이름, 월급 , 직무, 보너스를 출력하는데 직무가 SALESMAN 이고 월급이 1000 이상
--이면 보너스를 9000 을 출력하고 직무가 ANALYST 이고 월급이 2500 이상이면 보너
--스를 8000 을 출력하고 나머지 사원들은 0 을 출력하시오 !
select ename, sal, job
    , (case when (job = 'SALESMAN' and sal >= 1000) then 9000
            when (job = 'ANALYST' and sal >= 2500) then 8000
            else 0 end) bonus
from emp;
```

## 복수행 함수 (=group 함수)

```sql
--문제4-1. 사원 테이블에서 최대월급을 출력하시오 !
select max(sal)
from emp;
--문제4-2. 직무가 SALESMAN 인 사원들 중에서 최대월급을 출력하시오
select max(sal)
from emp
where job = 'SALESMAN';
--문제4-3. 부서번호, 부서번호별 최대월급을 출력하는데 부서번호별 최대월급이 높은것 부터
--출력하시오 !
select deptno, max(sal)
from emp
group by deptno
order by max(sal) desc;
--문제4-4. 위의 결과에서 부서번호 20번은 제외하고 출력하시오 !
select deptno, max(sal)
from emp
where deptno <> 20
group by deptno
order by max(sal) desc;
--문제4-5. 커미션의 평균값을 출력하시오 !
select avg(comm)
from emp;
--문제4-6. 위의 결과를 다시 출력하는데 전체 사원수로 나누게 하시오 !
select sum(comm)/count(nvl(comm,0))
from emp;
--문제4-7. 직무, 직무별 토탈월급을 출력하는데 직무가 SALESMAN 인 사원들을 제외하고 출력하
--고 직무별 토탈월급이 높은것부터 출력하는데 직무별 토탈월급을 출력할때에 천단위
--를 부여해서 출력하시오 ~
select job, to_char(sum(sal),'999,999') sum_sal
from emp
where job != 'SALESMAN'
group by job
order by sum(sal) desc;
--문제4-8. 부서번호, 부서번호별 토탈월급을 출력하시오 !
--DEPTNO SUM(SAL)
------------ ----------
--30 9400
--20 10875
--10 8750
select deptno, sum(sal)
from emp
group by deptno;
--문제4-9. 위의 결과를 가로로 출력하시오 ! (부서번호를 모두 안다고 가정)
--10 20 30
------------ ---------- ----------
--8750 10875 9400
select
    sum(decode(deptno, 10, sal)) as "10"
    , sum(decode(deptno, 20, sal)) as "20"
    , sum(decode(deptno, 30, sal)) as "30"
from emp;
--문제4-10. 직무, 직무별 토탈월급을 출력하시오 !
select job, sum(sal)
from emp
group by job;
--문제4-11. 위의 결과를 다시 출력하는데 직무별 토탈월급이
--5000 이상인것만 출력하시오 !
select job, sum(sal)
from emp
group by job
having sum(sal) >= 5000;
--문제4-12. 직무, 직무별 토탈월급을 출력하는데 직무가 SALESMAN 은 제외하고 출력하고 직무
--별 토탈월급이 4000 이상인것만 출력하고 직무별 토탈월급이 높은것부터 출력하시오 ~
select job, sum(sal) sum_sal
from emp
where job != 'SALESMAN'
group by job
having sum(sal) >= 4000
order by sum_sal desc;
--문제4-13. 사원 테이블의 전체 건수를 확인하시오 !
select count(*)
from emp;
--문제4-14. 직무, 직무별 인원수를 출력하시오 !
select job, count(job)
from emp
group by job;
--문제4-15. 직무, 직무별 평균월급을 출력하시오 !
select job, round(avg(sal))
from emp
group by job;
--문제4-16. 위의 결과중에 최대값을 출력하시오 !
select max(round(avg(sal)))
from emp
group by job;
--문제4-17. 직무, 부서번호, 직무별 부서번호별 토탈월급을
--출력하시오 !
--JOB 10 20 30
-------------------- ---------- ---------- ----------
--CLERK 1300 1900 950
--SALESMAN 5600
--PRESIDENT 5000
--MANAGER 2450 2975 2850
--ANALYST 6000
select job
    , sum(decode(deptno, 10, sal)) as "10"
    , sum(decode(deptno, 20, sal)) as "20"
    , sum(decode(deptno, 30, sal)) as "30"
from emp
group by job;
--문제4-18. 입사한 년도(4자리), 입사한 년도별 토탈월급을
--출력하시오 !
--TO_C SUM(SAL)
------ ----------
--1980 800
--1983 1100
--1982 4300
--1981 22825
select to_char(hiredate, 'RRRR'), sum(sal)
from emp
group by to_char(hiredate, 'RRRR');
--문제4-19. 위의 결과를 가로로 출력하시오 !
--1980 1981 1982 1983
------------ ---------- ---------- ----------
--800 22825 4300 1100
select
    sum(decode(to_char(hiredate, 'RRRR'), '1980', sal)) as "1980"
    , sum(decode(to_char(hiredate, 'RRRR'), '1981', sal)) as "1981"
    , sum(decode(to_char(hiredate, 'RRRR'), '1982', sal)) as "1982"
    , sum(decode(to_char(hiredate, 'RRRR'), '1983', sal)) as "1983"
from emp;
```