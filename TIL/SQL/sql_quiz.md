#SQL Quiz

```sql
--Q1. EMP 테이블에서 급여가 1500이상 3000이하이면서 커미션이 NULL이거나 0인 사원을 검색하세요
--
--     EMPNO ENAME      JOB              MGR HIREDATE        SAL       COMM     DEPTNO
------------ ---------- --------- ---------- -------- ---------- ---------- ----------
--      7566 JONES      MANAGER         7839 81/04/02       2975                    20
--      7698 BLAKE      MANAGER         7839 81/05/01       2850                    30
--      7782 CLARK      MANAGER         7839 81/06/09       2450                    10
--      7788 SCOTT      ANALYST         7566 82/12/09       3000                    20
--      7844 TURNER     SALESMAN        7698 81/09/08       1500          0         30
--      7902 FORD       ANALYST         7566 81/12/03       3000                    20
--
--6개 행이 선택되었습니다. 

select *
from emp
where sal between 1500 and 3000
and (comm is null or comm = 0);

--Q2. emp 테이블에서 12월에 입사한 사원을 입사일을 기준으로 검색하세요. 
--
--     EMPNO ENAME      JOB              MGR HIREDATE        SAL       COMM     DEPTNO
------------ ---------- --------- ---------- -------- ---------- ---------- ----------
--      7369 SMITH      CLERK           7902 80/12/17        800                    20
--      7902 FORD       ANALYST         7566 81/12/03       3000                    20
--      7900 JAMES      CLERK           7698 81/12/03        950                    30
--      7788 SCOTT      ANALYST         7566 82/12/09       3000                    20

select *
from emp
where hiredate between to_date('1980/12/01', 'YYYY//MM/DD')
                    and to_date('1981/01/01', 'YYYY//MM/DD') - 1/86400
or hiredate between to_date('1981/12/01', 'YYYY//MM/DD')
                    and to_date('1982/01/01', 'YYYY//MM/DD') - 1/86400
or hiredate between to_date('1982/12/01', 'YYYY//MM/DD')
                    and to_date('1983/01/01', 'YYYY//MM/DD') - 1/86400;

--Q3. EMP 테이블에서, 이름(ENAME)이 'S' 또는 'A'로 시작하는 사원을 검색하시오. 
--
--검색 결과
--
--     EMPNO ENAME             SAL     DEPTNO
------------ ---------- ---------- ----------
--      7788 SCOTT            3000         20
--      7369 SMITH             800         20
--      7876 ADAMS            1100         20
--      7499 ALLEN            1600         30

select *
from emp
where ename like 'S%' or ename like 'A%';

select *
from emp
where substr(ename, 1,1) in ('S', 'A');

--Q4. EMP 테이블에서, 급여(SAL)보다 커미션(COMM)을 많이 받는 사원을 검색하시오.
--
--검색 결과
--
--     EMPNO ENAME             SAL       COMM     DEPTNO
------------ ---------- ---------- ---------- ----------
--      7654 MARTIN           1250       1400         30

select *
from emp
where sal<comm;

--Q5. EMP 테이블에서, 급여(SAL)가 2000 보다 작고, 3000 보다 많은 사원을 검색하시오. 
--
--검색 결과
--
--     EMPNO ENAME             SAL     DEPTNO
------------ ---------- ---------- ----------
--      7369 SMITH             800         20
--      7876 ADAMS            1100         20
--      7499 ALLEN            1600         30
--      7521 WARD             1250         30
--      7654 MARTIN           1250         30
--      7844 TURNER           1500         30
--      7900 JAMES             950         30
--      7934 MILLER           1300         10
--      7839 KING             5000         10

select *
from emp
where sal < 2000 or sal >3000;

--Q6. EMP 테이블에서, 커미션(COMM)을 기준으로 내림차순 정렬된 결과를 검색하시오.
--단, 커미션이 NULL인 행은 마지막에 검색되도록 한다. 
--
--검색 결과
--
--     EMPNO ENAME             SAL       COMM
------------ ---------- ---------- ----------
--      7654 MARTIN           1250       1400
--      7521 WARD             1250        500
--      7499 ALLEN            1600        300
--      7844 TURNER           1500          0
--      7839 KING             5000
--      7788 SCOTT            3000
--      7782 CLARK            2450
--      7900 JAMES             950
--      7902 FORD             3000
--      7934 MILLER           1300
--      7876 ADAMS            1100
--      7369 SMITH             800
--      7566 JONES            2975
--      7698 BLAKE            2850

select *
from emp
order by comm desc nulls last;

select *
from emp
order by nvl(comm,-1) desc;

--Q7. EMP 테이블에서, 매니저(MGR)가 없는 사원은 'NO MANAGER'의 문자를 다음과 같이 검색하시오.
--
--검색 결과
--
--     EMPNO ENAME      JOB             MANAGER
------------ ---------- --------------- ---------------
--      7788 SCOTT      ANALYST         7566
--      7566 JONES      MANAGER         7839
--      7369 SMITH      CLERK           7902
--      7876 ADAMS      CLERK           7788
--      7499 ALLEN      SALESMAN        7698
--      7521 WARD       SALESMAN        7698
--      7654 MARTIN     SALESMAN        7698
--      7698 BLAKE      MANAGER         7839
--      7782 CLARK      MANAGER         7839
--      7844 TURNER     SALESMAN        7698
--      7900 JAMES      CLERK           7698
--      7902 FORD       ANALYST         7566
--      7934 MILLER     CLERK           7782
--      7839 KING       PRESIDENT       NO Manager

select empno, ename, job, nvl(to_char(mgr), 'NO Manager')
from emp;

--Q8. EMP 테이블에서 DEPTNO, JOB 컬럼으로 Grouping 된 급여의 합계를 다음과 같이 검색하시오.
--
--검색 결과
--
--    DEPTNO    ANALYST      CLERK    MANAGER  PRESIDENT   SALESMAN
------------ ---------- ---------- ---------- ---------- ----------
--        10                  1300       2450       5000
--        20       6000       1900       2975
--        30                   950       2850                  5600

select deptno
    , sum(decode(job,'ANALYST',sal)) ANALYST
    , sum(decode(job,'CLERK',sal)) CLERK
    , sum(decode(job,'MANAGER',sal)) MANAGER
    , sum(decode(job,'PRESIDENT',sal)) PRESIDENT
    , sum(decode(job,'SALESMAN',sal)) SALESMAN
from emp
group by deptno

--Q9. EMP 테이블에서, 'JONES' (ENAME)보다 더 많은 급여(SAL)를 받는 사원을 검색하시오.
--단, JONES의 급여도 함께 검색합니다.
--
--검색 결과 
--
--     EMPNO ENAME             SAL Jones's Salary
------------ ---------- ---------- --------------
--      7788 SCOTT            3000           2975
--      7902 FORD             3000           2975
--      7839 KING             5000           2975

select empno, ename, sal, (select sal
                from emp
                where ename = 'JONES') "Jones's Salary"
from emp
where sal > (select sal
                from emp
                where ename = 'JONES');

--Q10. DEPT, EMP 테이블을 사용하여 부서별 급여의 합계를 다음과 같이 검색하시오. 
--근무하는 사원이 없는 부서도 함께 표시합니다. 
--
--검색 결과 
--
--    DEPTNO DNAME             SUM_SAL
------------ -------------- ----------
--        10 ACCOUNTING           8750
--        20 RESEARCH            10875
--        30 SALES                9400
--        40 OPERATIONS         (null)      

select d.deptno, d.dname, sum(e.sal) sum_sal
from dept d, emp e
where d.deptno=e.deptno(+)
group by d.deptno, d.dname
order by d.deptno;

--Q11. DEPT, EMP 테이블을 사용하여 각 부서의 소속 사원 유무를 확인하는 검색 결과를 만드시오.
--EMP 컬럼은 소속 사원이 존재할 때 'YES', 아니면 'NO'를 검색합니다. 
--
--검색 결과
--
--    DEPTNO DNAME          LOC           EMP
------------ -------------- ------------- ---
--        10 ACCOUNTING     NEW YORK      YES
--        20 RESEARCH       DALLAS        YES
--        30 SALES          CHICAGO       YES
--        40 OPERATIONS     BOSTON        NO

select d.deptno, d.dname, d.loc
    , decode(count(e.deptno), 0,'NO','YES') emp
from dept d, emp e
where d.deptno=e.deptno(+)
group by d.deptno, d.dname, d.loc
order by d.deptno;

--스칼라 서브쿼리 사용, 위와 출력 동일
select d.*, nvl((select 'YES'
                 from emp
                 where deptno = d.deptno
                 and rownum = 1)         ,'NO') as emp
from dept d;

--exists 구문 사용
select d.*, nvl((select 'YES'
                 from dual
                 where exists (select 1
                               from emp
                               where deptno = d.deptno)),'NO') as emp
from dept d;

--Q12. COUNTRIES, EMPLOYEES 테이블을 이용하여, 'Canada'에서 근무 중인 사원 정보를 다음과 같이 검색하시오. 만약 추가적으로 필요한 테이블이 더 있다면 함께 사용합니다. 
--
--검색 결과
--
--FIRST_NAME      LAST_NAME           SALARY JOB_ID     COUNTRY_NAME
----------------- --------------- ---------- ---------- ---------------
--Michael         Hartstein            13000 MK_MAN     Canada
--Pat             Fay                   6000 MK_REP     Canada

select e.first_name, e.last_name, e.salary, e.job_id, c.country_name
from countries c, employees e, departments d, locations l
where e.department_id = d.department_id
and d.location_id = l.location_id
and l.country_id = c.country_id
and c.country_name = 'Canada';

--Q13. EMP 테이블에서 1981년도에 입사한 사원들을 입사 월별로 인원수를 검색하시오. 
--단, 사원이 없는 월도 함께 출력 
--
--검색 결과
--
--HIRE                 CNT
--------------- ----------
--1981/01                0
--1981/02                2
--1981/03                0
--1981/04                1
--1981/05                1
--1981/06                1
--1981/07                0
--1981/08                0
--1981/09                2
--1981/10                0
--1981/11                1
--1981/12                2

select b.hire, nvl(a.cnt,0)
from (select to_char(hiredate, 'YYYY/MM') hire, count(*) cnt
        from emp
        where to_char(hiredate, 'YYYY') = 1981
        group by to_char(hiredate, 'YYYY/MM')) a,

    (select '1981/'||lpad(level,2,0) hire
        from dual
        connect by level <=12) b
where a.hire(+) = b.hire
order by 1;
```