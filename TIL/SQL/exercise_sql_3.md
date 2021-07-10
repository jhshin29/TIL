#Join

```sql
--문제5-1. 이름과 부서위치를 출력하시오 !
select e.ename, d.loc
from emp e join dept d
on e.deptno = d.deptno;
--문제5-2. 위의 결과를 다시 출력하는데 부서위치가 DALLAS 인 사원들만 출력하시오 !
select e.ename, d.loc
from emp e, dept d
where e.deptno = d.deptno
and d.loc = 'DALLAS';
--문제5-3. 직업이 SALESMAN 인 사원들의 이름과 월급과 직업과 부서위치를 출력하시오 !
select e.ename, e.sal, e.job, d.loc
from emp e, dept d
where e.deptno = d.deptno
and e.job = 'SALESMAN';
--문제5-4. 위의 결과를 다시 출력하는데 이름과 월급과 직업과 부서위치, 부서번호를 출력하시
--오 !
select e.ename, e.sal, e.job, d.loc, d.deptno
from emp e, dept d
where e.deptno = d.deptno
and e.job = 'SALESMAN';
--문제5-5. 월급이 3000 이상인 사원들의 이름과 월급과 부서위치를 출력하시오 !
select e.ename, e.sal, d.loc
from emp e join dept d
on e.deptno = d.deptno
and e.sal >= 3000;
--문제5-6. 직업이 SALESMAN 인 사원들의 이름과 직업과 월급과 부서위치를 출력하시오 !
select e.ename, e.job, e.sal, d.loc
from emp e, dept d
where e.deptno = d.deptno
and e.job = 'SALESMAN';
--문제5-7. 부서위치, 이름, 월급, 순위를 출력하는데 순위가 월급이 높은 순서대로 순위를 출
--력하시오 !
select d.loc, e.ename, e.sal, j.grade
from emp e, dept d, salgrade j
where e.deptno = d.deptno
and e.sal between j.losal and j.hisal
order by sal desc;
```