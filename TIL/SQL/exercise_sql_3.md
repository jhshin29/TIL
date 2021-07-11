```sql
--문제6-1. 최대월급을 받는 사원의 이름과 월급을 출력하시오 !
select ename, sal
from emp
where sal = (select max(sal)
            from emp);
--문제6-2. JONES 보다 더 많은 월급을 받는 사원들의 이름과 월급을 출력하시오 !
select ename, sal
from emp
where sal > (select sal
            from emp
            where ename = 'JONES');
--문제6-3. SCOTT 과 같은 월급을 받는 사원들의 이름과 월급을 출력하시오 !
select ename, sal
from emp
where sal = (select sal
            from emp
            where ename = 'SCOTT');
--문제6-4. ALLEN 보다 늦게 입사한 사원들의 이름과 입사일을 출력하시오 !
select ename, hiredate
from emp
where hiredate > (select j.hiredate
            from emp j
            where j.ename = 'ALLEN');
--문제6-5. 사원 테이블에 최대월급을 받는 사원의 이름과 월급을 출력하시오 !
select ename, sal
from emp
where sal = (select max(sal)
            from emp);
--문제6-6. ALLEN 과 같은 부서번호에서 근무하는 사원들의 이름과 부서번호를 출력하시오 !
select ename, deptno
from emp
where deptno = (select deptno
            from emp
            where ename = 'ALLEN');
--문제6-7. SCOTT 과 같은 직업을 갖는 사원들의 이름과 직업을 출력하는데 SCOTT 은 제외하고
--출력하시오 !
select ename, job
from emp
where job = (select job
            from emp
            where ename = 'SCOTT')
and ename <> 'SCOTT';
--문제6-8. 직속 상사가 KING인 사원들을 출력하시오!
select ename
from emp
where mgr = (select empno
            from emp
            where ename = 'KING');
--문제6-9. DALLAS 에 있는 부서번호에서 근무하는 사원들의 이름과 월급을 출력하시오 !
--(조인 쓰지 말고 서브쿼리로만)
select ename, sal
from emp
where deptno = (select deptno
                    from dept
                    where loc = 'DALLAS');
--문제6-10. 직업이 SALESMAN 인 사원들과 월급이 같은 사원들의 이름과 월급을 출력하시오 !
select ename, sal
from emp
where sal in (select sal
            from emp
            where job = 'SALESMAN');
--문제6-11. 직업이 SALESMAN 인 사원들과 월급이 같지 않은 사원들의 이름과 월급을 출력하시오!
select ename, sal
from emp
where sal not in (select sal
            from emp
            where job = 'SALESMAN');
--문제6-12. 관리자인 사원들의 이름을 출력하시오!
select ename
from emp
where empno in (select distinct mgr
                from emp
                where mgr is not null);
--exists 구문 사용
select e.empno, e.ename
from emp e
where exists (select *
                from emp e1
                where e1.mgr = e.empno);
--문제6-13. 관리자가 아닌 사원들의 이름을 출력하시오 ! (즉, 관리자가 아닌 사원)
select ename
from emp
where empno not in (select distinct mgr
                    from emp
                    where mgr is not null);

select e.empno, e.ename
from emp e
where not exists (select *
                from emp e1
                where e1.mgr = e.empno);
--문제6-14. 직업이 SALESMAN 인 사원들중에서의 최대월급보다 더 많은 월급을 받는 사원들의
--이름과 월급을 출력하시오 !
select ename, sal
from emp
where sal > (select max(sal)
                from emp
                where job = 'SALESMAN');

select ename, sal
from emp
where sal > all ( select sal
                from emp
                where job='SALESMAN' );
--문제6-15. 직업이 SALESMAN 인 사원들중에서 가장 작은 월급을 받는 사원보다 더 큰 월급을
--받는 사원들의 이름과 월급을 출력하시오 !
select ename, sal
from emp
where sal > (select min(sal)
                from emp
                where job = 'SALESMAN');

select ename, sal
from emp
where sal > any ( select sal
                from emp
                where job='SALESMAN' );
--문제6-16. 30번 부서번호인 사원들 중에서 가장 늦게 입사한 사원보다 더 늦게 입사한 사원들
--의 이름과 입사일을 출력하시오 ! (전체 사원을 대상으로 수행)
select ename, hiredate
from emp
where hiredate > (select max(hiredate)
                from emp
                where deptno = '30');
--문제6-17. 부서 테이블에서 부서번호와 부서위치를 출력하는데 사원 테이블에 존재하는 부서
--번호에 대한 것만 출력하시오 !
select deptno, loc
from dept
where deptno in (select distinct deptno
                from emp);

select d.deptno, d.loc
from dept d
where exists (select 1
                from emp e
                where e.deptno = d.deptno);
--문제6-18. 부서테이블에서 부서번호와 부서위치를 출력하는데 사원 테이블에 존재하지 않는
--것을 출력하시오 !
select deptno, loc
from dept
where deptno not in (select distinct deptno
                from emp);

select d.deptno, d.loc
from dept d
where not exists (select 1
                from emp e
                where e.deptno = d.deptno);
```