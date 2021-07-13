#SQL ROLLUP CUBE PROB

```sql
--Q1
--문제 1.
--DEPTNO HD_YYYY JOB SUM(SAL)
------------ -------- ---------- ----------
--20 1980 CLERK 800
--1980 CLERK 800
--30 1981 CLERK 950
--1981 CLERK 950
--10 1982 CLERK 1300
--1982 CLERK 1300
--20 1983 CLERK 1100
--1983 CLERK 1100
--20 1981 ANALYST 3000
--1981 ANALYST 3000
--20 1982 ANALYST 3000
--DEPTNO HD_YYYY JOB SUM(SAL)
------------ -------- ---------- ----------
--1982 ANALYST 3000
--10 1981 MANAGER 2450
--20 1981 MANAGER 2975
--30 1981 MANAGER 2850
--1981 MANAGER 8275
--30 1981 SALESMAN 5600
--1981 SALESMAN 5600
--10 1981 PRESIDENT 5000
--1981 PRESIDENT 5000
--29025
--21 rows selected.

select deptno, to_char(hiredate, 'YYYY') hd_yyyy ,job, sum(sal)
from emp
group by rollup((job, to_char(hiredate, 'YYYY')),deptno)
order by 2;

--문제 2.
--DEPTNO HD_YYYY SU_SAL
------------ -------- ----------
--10 1981 7450
--10 1982 1300
--10 8750
--20 1980 800
--20 1981 5975
--20 1982 3000
--20 1983 1100
--20 10875
--30 1981 9400
--30 9400
--29025
--11 rows selected.

select deptno, to_char(hiredate, 'YYYY') hd_yyyy , sum(sal) su_sal
from emp
group by rollup(deptno, to_char(hiredate, 'YYYY'));

--문제 3.
--DEPTNO HD_YYYY JOB SU_SAL
------------ -------- ---------- ----------
--10 1982 CLERK 1300
--20 1980 CLERK 800
--20 1983 CLERK 1100
--30 1981 CLERK 950
--CLERK 4150
--20 1981 ANALYST 3000
--20 1982 ANALYST 3000
--ANALYST 6000
--10 1981 MANAGER 2450
--20 1981 MANAGER 2975
--30 1981 MANAGER 2850
--DEPTNO HD_YYYY JOB SU_SAL
------------ -------- ---------- ----------
--MANAGER 8275
--30 1981 SALESMAN 5600
--SALESMAN 5600
--10 1981 PRESIDENT 5000
--PRESIDENT 5000
--16 rows selected.

select deptno, to_char(hiredate, 'YYYY') hd_yyyy ,job, sum(sal)
from emp
group by job,rollup( (to_char(hiredate, 'YYYY'), deptno));

--문제 4.
--DEPTNO HD_YYYY JOB SU_SAL
------------ -------- ---------- ----------
--20 1980 CLERK 800
--1980 CLERK 800
--30 1981 CLERK 950
--1981 CLERK 950
--10 1982 CLERK 1300
--1982 CLERK 1300
--20 1983 CLERK 1100
--1983 CLERK 1100
--20 1981 ANALYST 3000
--1981 ANALYST 3000
--20 1982 ANALYST 3000
--DEPTNO HD_YYYY JOB SU_SAL
------------ -------- ---------- ----------
--1982 ANALYST 3000
--10 1981 MANAGER 2450
--20 1981 MANAGER 2975
--30 1981 MANAGER 2850
--1981 MANAGER 8275
--30 1981 SALESMAN 5600
--1981 SALESMAN 5600
--10 1981 PRESIDENT 5000
--1981 PRESIDENT 5000
--20 rows selected.
select deptno, to_char(hiredate, 'YYYY') hd_yyyy ,job, sum(sal)
from emp
group by job,to_char(hiredate, 'YYYY'),rollup(deptno);

--문제 5.
--DEPTNO HD_YYYY SU_SAL
------------ -------- ----------
--29025
--1980 800
--1981 22825
--1982 4300
--1983 1100
--10 8750
--10 1981 7450
--10 1982 1300
--20 10875
--20 1980 800
--20 1981 5975
--DEPTNO HD_YYYY SU_SAL
------------ -------- ----------
--20 1982 3000
--20 1983 1100
--30 9400
--30 1981 9400
--15 rows selected.
select deptno, to_char(hiredate, 'YYYY') hd_yyyy , sum(sal)
from emp
group by cube(to_char(hiredate, 'YYYY'),deptno)
order by 1 asc nulls first, 2 desc;

--문제 6.
--DEPTNO HD_YYYY JOB SU_SAL
------------ -------- ---------- ----------
--10 1982 CLERK 1300
--20 1980 CLERK 800
--20 1983 CLERK 1100
--30 1981 CLERK 950
--CLERK 4150
--20 1981 ANALYST 3000
--20 1982 ANALYST 3000
--ANALYST 6000
--10 1981 MANAGER 2450
--20 1981 MANAGER 2975
--30 1981 MANAGER 2850
--DEPTNO HD_YYYY JOB SU_SAL
------------ -------- ---------- ----------
--MANAGER 8275
--30 1981 SALESMAN 5600
--SALESMAN 5600
--10 1981 PRESIDENT 5000
--PRESIDENT 5000
--16 rows selected.

select deptno, to_char(hiredate, 'YYYY') hd_yyyy ,job, sum(sal)
from emp
group by job, cube((to_char(hiredate, 'YYYY'),deptno));

--문제 7.
--DEPTNO HD_YYYY SU_SAL
------------ -------- ----------
--30 1981 9400
--20 1981 5975
--20 1982 3000
--20 1983 1100
--20 1980 800
--10 1981 7450
--10 1982 1300
--7 rows selected.
select deptno, to_char(hiredate, 'YYYY') hd_yyyy , sum(sal)
from emp
group by (to_char(hiredate, 'YYYY'),deptno) --괄호가 있고 없고 차이
order by 1 desc;

select deptno, to_char(hiredate, 'YYYY') hd_yyyy , sum(sal)
from emp
group by to_char(hiredate, 'YYYY'),deptno
order by 1 desc;

--문제 8.
--DEPTNO HD_YYYY JOB SUM(SAL)
------------ -------- ---------- ----------
--20 1980 CLERK 800
--30 1981 CLERK 950
--10 1982 CLERK 1300
--20 1983 CLERK 1100
--20 1981 ANALYST 3000
--20 1982 ANALYST 3000
--10 1981 MANAGER 2450
--20 1981 MANAGER 2975
--30 1981 MANAGER 2850
--30 1981 SALESMAN 5600
--10 1981 PRESIDENT 5000
--DEPTNO HD_YYYY JOB SUM(SAL)
------------ -------- ---------- ----------
--1980 CLERK 800
--1981 CLERK 950
--1982 CLERK 1300
--1983 CLERK 1100
--1981 ANALYST 3000
--1982 ANALYST 3000
--1981 MANAGER 8275
--1981 SALESMAN 5600
--1981 PRESIDENT 5000
--10 8750
--20 10875
--DEPTNO HD_YYYY JOB SUM(SAL)
------------ -------- ---------- ----------
--30 9400
--29025
--24 rows selected.
select deptno, to_char(hiredate, 'YYYY') hd_yyyy ,job, sum(sal)
from emp
group by cube((job, to_char(hiredate, 'YYYY')),deptno);
```