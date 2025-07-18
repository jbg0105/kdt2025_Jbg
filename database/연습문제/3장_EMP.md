#3장_EMP
```
use exam;

#1.dept 테이블 생성
create table dept(
deptno int not null primary key,
dname varchar(14),
loc varchar(13)
);

#2.emp 테이블 생성
create table emp(
empno int not null primary key,
ename varchar(10),
job varchar(9),
mgr int,
hiredate date,
sal int,
comm int,
deptno int,
foreign key (deptno) references dept(deptno)
);

#3. dept에 데이터 삽입
insert into dept(deptno, dname, loc) values
(10, 'ACCOUNTING', 'NEW YORK'),
(20, 'RESEARCH', 'DALLAS'),
(30, 'SALES', 'CHICAGO'),
(40, 'OPERATIONS', 'BOSTON');

#4. emp에 데이터 삽입
insert into emp(empno, ename, job, mgr, hiredate, sal, comm, deptno) values
(7369, 'SMITH', 'CLERK', 7902, '1920-12-17', 800, null, 20),
(7499, 'ALLEN', 'SALESMAN', 7698, '1981-02-20', 1600, 300, 30),
(7521, 'WARD', 'SALESMAN', 7698, '1981-02-22', 1250, 500, 30),
(7566, 'JONES', 'MANAGER', 7839, '1981-04-02', 2975, null, 20);

select * from dept;
select * from emp;

#5. 오류메시지 확인 및 원인
INSERT INTO Emp(empno, ename, job, mgr, hiredate, sal, comm, deptno) 
  VALUES (7654, 'MARTIN', 'SALESMAN', 7698, '1981-09-28 00:00:00', 1250, 1400, 50);
  -- deptno=50이 dept 테이블에 없기 때문에 외래키 제약 조건에 오류가 발생함(외래키가 참조를 못함..?)
  
#6. 사원의 이름과 근무지역을 출력
select e.ename, d.loc
from emp e
join dept d on e.deptno=d.deptno;

 
 #7. alter, update 사용
 alter table dept add managername varchar(20);

update dept set managername='SMITH' where deptno=10;
update dept set managername='ALLEN' where deptno=20;
update dept set managername='WARD' where deptno=30;
update dept set managername='JONES' where deptno=40;
```
