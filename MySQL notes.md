#### MySQL-关系型数据库

命令行中打开mysql工具：mysql -u root -p

退出mysql：exit;



#### ※常用数据类型：

MySQL支持多种类型，大致可分为三类：

**数值：**TINYINT、SMALLINT、MEDIUMINT、**INT**、BIGINT、**FLOAT、DOUBLE**、DECIMAL

**日期/时间：DATE、TIME、YEAR、DATETIME**、TIMESTAMP

**字符串：CHAR、VARCHAR**、TINYBLOB、TINYTEXT、BLOB、TEXT、MEDIUMBLOB、MEDIUNTEXT、LONGBLOB、LONGTEXT

数据类型的选择：日期按照格式、数值和字符串按照大小



#### **※如何操作数据库：**

create databse 数据库名；创建数据库

show databases；显示全部数据库

drop database 数据库名；删除数据库

use 数据库名；采用某个数据库



#### **※如何操作数据表：**

create table 表名；创建数据表，需同时创建字段

create table class (id int(4) primary key auto_increment, 

​								name char(20) not null);

show tables；显示数据库下的全部数据表

describe 表名； 查看已创建的数据表结构

show create table 表名；查看表结构<表内字段及类型>	结尾加\G，格式清楚显示

drop table 表名；删除数据表



##### 建表约束：

**主键约束primary key：**给某个字段添加主键约束，可使得**该字段内不重复且不为空**，因此能唯一确定表中记录；同一表中可添加多个主键<联合主键>，primary key(id,name)，二者不可同时重复，但均不能为空；
		建表之后添加主键：alter table 表名 add primary key(id);
		建表之后删除主键：alter table 表名 drop primary key;
		建表之后修改主键：alter table 表名 modify id int primary key;

**自增约束auto_increment：**和主键约束搭配使用，自动管控对应主键，无输入时自动增长

**唯一约束unique：**约束修饰的字段的值不可以重复<可为空>	若添加两个唯一约束，则二者不同时重复即可
		alter table 表名 add unique(name);	

**非空约束not null：**修饰的字段不能为空

**默认约束default：**插入数据时，相应字段没有传值，则使用默认值

**外键约束foreign key：**涉及到两个表-主表、副表	若主表记录被副表引用，则不可以删除
		foreign key(class_id) references classes(id)



##### 设计范式：

1.第一范式-1NF：数据表中的所有字段都是不可分割的原子值(如详细地址是可分割的)

2.第二范式-2NF：满足第一范式的前提下，除主键外的每一列都必须完全依赖于主键；有联合主键的情况下，会出现不完全依赖，需对表进行拆分

3.第三范式-3NF：满足第二范式的前提下，除主键列的其他列之间不能有传递依赖关系



#### **※如何操作数据表中的数据<增删改查>：**

insert into 表名 values(内容)；插入数据

delete from 表名 where 条件；删除数据

update 表名 set 字段=新值 where 条件；更新、修改数据

select * from 表名 where 条件；查询数据



#### **※连接查询：**

**person表** (id, name, cardID)								**card表** (id, name)

SQL有四种连接查询：

**内连接** <u>inner join或者join</u>  

​			<两张表中数据-通过某个字段相等-查询出相关记录数据>；

​			select * from person <u>**inner join**</u> card **on person.cardid=card.id**;

**外连接** ①**左连接** <u>left join 或者 left outer join</u> 

​			<以左表数据全部显示为基准，右表有对应则显示，无对应则为NULL>

​			select * from person **<u>left join</u>** card **on person.cardid=card.id**;

**外连接** ②**右连接** <u>right join 或者 right outer join</u>

​			<以右表数据全部显示为基准，左表有对应则显示，无对应则为NULL>

​			select * from person **<u>right join</u>** card **on person.cardid=card.id**;

**外连接** ③**完全外连接** full join 或者 full outer join

​			<MySQL不支持全连接，需通过union将左、右连接合并来实现>

​			select * from person left join card on person.cardid=card.id
​			union
​			select * from person right join card on person.cardid=card.id;



#### **※查询语句：**

##### 详解：

**student表** (学号sno、姓名sname、性别ssex、出生日期sbirthday、班级class)

**teacher表** (教师编号tno、教师名称tname、性别tsex、出生日期tbirthday、职称prof、部门depart)

**course表** (课程号cno、课程名称cname、* 教师编号tno)

**score表** ( * 学号sno、* 课程号cno、分数degree)

**grade表** (最低low、最高upp、等级rank)



1.查询表中所有记录<全部字段>——select ***** from student;

2.查询指定字段—— select **sname, ssex, class** from student;

3.查询同一字段全部取值<无重复>——select **distinct** depart from teacher; <加入distinct关键字排重复>

4.查询字段取值在某一区间的全部记录——select * from score **where** degree **between** 60 **and** 80; 

<between-and关键字 或者 运算符>   		 select * from score **where** degree **>** 60 and degree **<** 80;

5.查询字段取某些值时的记录<in表示或者>——select * from score where degree **in**(85, 86, 88);

6.通过不同字段限制查询条件——select * from student where class='95031' **or** ssex='女';

7.根据某字段升序<asc,默认升序>、降序查询< desc >——select * from student **order by** class **desc**;

8.同时根据某字段升序、某字段降序查询——select * from score **order by cno asc, degree desc**;

9.统计某字段满足条件的记录总数——select **count(*)** from student where class = '95031';

10.查询score表中最高分的学生学号和课程号——

<子查询，**唯一选项用=**>select sno, cno from score where degree **= (select max(degree) from score)**;

<排序，最高分不止一个时有缺陷>	<limit 第一个数字表示查询起始位置，第二个数字表示查询数量>

select sno,cno from score order by degree desc **limit 0,1**;



11.查询一门课的平均成绩——select **avg(degree)** from score where cno='3-105';

查询每门课的平均成绩<group by-分组>——select cno,avg(degree) from score **group by cno**;

12.查询score表中至少有2名学生选修的并以3开头的课程的平均分数<where对行过滤，having对分组过滤>：

select cno, avg(degree) from score group by cno **having** count(cno)>=2 and cno **like '3%'**;

13.查询分数大于70，小于90的sno列——select sno,degree from score where degree between 70 and 90;

14.同时查询学生的sname、cno和degree列<多表查询>——

select sname, cno, degree from **student, score** where **student.sno=score.sno**;

15.查询所有学生的sno、cname和degree列<多表查询>——

select sno, cname, degree from **score, course** where **score.cno=course.cno**;

16.查询所有学生的sname、cname和degree列<三表查询>——

select sname, cname, degree, **score.sno as sco_sno, score.cno** from score, course, student where **score.sno = student.sno and score.cno = course.cno**;

17.查询“95031”班学生每门课的平均分<**多个选项用in**>——select cno, avg(degree) from score where sno **in** (select sno from student where class='95031') **group by cno**;

18.查询选修“3-105”课程的成绩高于“109”号同学的“3-105”成绩的所有同学记录——select * from score where cno="3-105" and degree>(select degree from score where sno='109'and cno='3-105');

19.查询成绩高于学号为“109”、课程号为“3-105”的成绩的所有记录——

select * from score where degree > (select degree from score where sno='109' and cno='3-105');

20.查询和学号108、101的同学同年出生的所有学生的sno、sname和sbirthday列——

select sno, sname, sbirthday from student where **year(sbirthday)** in (select year(sbirthday) from student where sno in (108,101));



21.查询“张旭”教师任课的学生成绩<多层嵌套子查询>——select degree from score **where** cno = (select cno from course **where** tno = (select tno from teacher where tname = "张旭"));

22.查询选修某课程的同学人数多于5人的教师姓名——select tname from teacher where tno in (select tno from course where cno in (select cno from score **group by cno having count(*)>5**));

23.查询95033班和95031班全体学生的记录——select * from student where **class in ('95031', '95033')**;

24.查询存在有85分以上成绩的课程cno——select distinct cno from score where degree > 85;

25.查询出“计算机系”教师所教课程的成绩表——select * from score where cno in (select cno from course where tno in (select tno from teacher where depart='计算机系'));

26.*查询“计算机系”与“电子工程系”不同职称的教师的tname和prof<union求并集>——

select * from teacher where depart='计算机系' and prof **not in** (select prof from teacher where depart ='电子工程系') **union** select * from teacher where depart='电子工程系' and prof **not in** (select prof from teacher where depart ='计算机系');

27.查询选修“3-105”课程的且成绩**至少高于**<大于其中至少一个，any关键字>选修“3-245”课程的成绩的cno、sno、degree，并按degree降序排列——select * from score where cno='3-105' and **degree>any**(select degree from score where cno='3-245') order by degree desc;

28.查询选修“3-105”课程的且其成绩高于选修“3-245”课程的全体同学的cno、sno和degree——

select * from score where cno='3-105' and degree > **all** (select degree from score where cno = '3-245');

29.查询所有教师和同学的name、sex和birthday<as关键词为结果列取别名，第二个表默认不用取别名>——

select sname **as** name, ssex **as** sex, sbirthday **as** birthday from student **union** select tname, tsex, tbirthday from teacher;

30.查询所有女教师和女学生的name、sex和birthday——select sname as name, ssex as sex, sbirthday as birthday from student **where ssex**='女' union select tname, tsex, tbirthday from teacher **where tsex**='女';



31.查询成绩比该课程平均成绩低的同学的成绩表<复制表做条件查询>——select * from score **a** where degree < (select avg(degree) from score **b where a.cno=b.cno**);

32.查询所有**任课老师**的tname和depart——

select tname, depart from teacher where tno in (select tno from course);

33.查询至少有2名男生的班号<条件加分组筛选>——

select class from student **where** ssex='男' **group by** class **having** count(*)>1;

34.查询student表中不姓王的同学记录<模糊查询取反>——select * from student where sname **not like** '王%';

35.查询student表中每个学生的姓名和年龄——

select sname, **year(now())-year(sbirthday) as age** from student;

36.查询student表中最大和最小的sbirthday日期值<最大最小与常识理解相反>——select **max**(sbirthday) as max, **min**(sbirthday) as min from student;

37.以班号和年龄从大到小的顺序查询student表中的全部记录——select * from student **order by class desc, sbirthday**;

38.*查询男教师及其所上的课程——

select * from course where tno in (select tno from teacher where tsex='男');

39.查询最高分同学的sno、cno和degree列——

select * from score where degree = (select **max(degree)** from score);

40.查询和李军同性别的所有同学的sname——

select sname from student where ssex = (select ssex from student where sname='李军');

41.查询和李军同性别**且**同班的同学sname——select sname from student where ssex = (select ssex from student where sname='李军') **and** class = (select class from student where sname='李军');

42.查询所有选修计算机导论课程的男同学的成绩表——select * from score where cno = (select cno from course where cname='计算机导论') **and** sno in (select sno from student where ssex='男');

43.新建grade表，查询所有同学的sno、cno和**rank**列——

select sno,cno,rank from score,grade where **degree between low and upp**;



##### 难题练习：

**dept表** (部门编号deptno、部门名称dname、部门地点dloc)

**emp表** (员工编号empno、员工名称ename、职位job、直接上级mgr、雇佣日期hiredate、薪金sal、comm、部门编号deptno)

**1.查询出至少有一个员工的部门，显示部门的编号(deptno)，部门名称(dname)，部门位置(dloc)，部门人数**
select d.\*, e.cnt from 
dept **as** d **inner join** (select deptno, count(\*) as cnt from emp group by deptno) **as** e on d.deptno=e.deptno;

**2.列出所有员工及其直接上级的姓名**
IFNULL() 函数--第一个表达式为 NULL时返回第二个参数的值，如果不为 NULL则返回第一个参数的值。
select e.ename, **ifnull**(m.ename, 'boss') as super from 
emp as e **left outer join** emp as m on e.mgr = m.empno;

**3.列出受雇日期早于直接上级的所有员工的编号、姓名、部门名称<两重连接查询>**
①select l.empno, l.ename,d.dname from dept as d join (select e.empno, e.ename, e.deptno from emp as e join emp as m on e.mgr = m.empno and e.hiredate < m.hiredate) as l on d.deptno=l.deptno

②▲ select e.empno, e.ename, d.dname from emp as e, emp as m, dept as d where e.mgr = m.empno and e.hiredate < m.hiredate and e.deptno = d.deptno

**4.列出部门名称和这些部门的员工信息，<u>同时</u>列出那些没有员工的部门**
select * from dept as d left join emp as e on d.deptno=e.deptno

**5.列出<u>最低薪金</u>大于1500的各种工作及从事此工作的员工人数**
select job, count(*) from emp group by job **having min(sal)** > 1500

**6.列出在SALES部工作员工的姓名，假设不知道SALES部门的部门编号**
select ename from emp where deptno = (select deptno from dept where dname = 'SALES');

**7.列出部门名称及该部门人数**
select d.dname, t.cnt from dept as d join (select deptno, count(*) as cnt from emp group by deptno) as t on d.deptno = t.deptno

**8.列出与SMITH从事相同工作的所有员工及部门名称**
select e.*, d.dname from emp as e, dept as d where job = (select job from emp where ename = 'SMITH') and e.deptno=d.deptno

**9.列出薪金高于在部门30工作的所有员工的薪金的员工姓名和薪金、部门名称**
①select t.ename, t.sal, d.dname from dept as d join (select * from emp as e where e.sal > **all**(select sal from emp where deptno='30')) as t where t.deptno = d.deptno

②▲ select e.ename, e.sal, d.dname from emp as e, dept as d where d.deptno=e.deptno and e.sal>all(select sal from emp where deptno='30')



#### **※mysql事务：**

事务：最小的不可分割的工作单元，能够保证一个业务的完整性。提供反悔机会
例如-多条sql语句，可能会有同时成功的要求，或者同时失败

事务的控制：
mysql默认开启事务(自动提交)，执行语句时，效果立即体现，且不能回滚<rollback，撤销sql语句执行效果>

关闭自动提交：
set autocommit = 0；插入语句可临时生效，但可回滚；插入后手动commit提交，回滚失效<持久性>

begin; start transaction;	在默认自动提交前提下，可开启一个手动提交的事务，可回滚，但提交后回滚失效



**事务的四大特征 ACID：**

A 原子性Atomicity	事务是最小的单位，不可再分割

C 一致性Consistency	事务要求，同一事务中的sql语句，必须保证同时成功或失败

I 隔离性Isolation	事务之间具有隔离性

D 持久性Durability	事务一旦结束(提交或回滚即为结束)，不可返回



**事务的隔离性：**

修改隔离级别：set global transaction isolation level <u>read committed</u>

1.read uncommitted-读未提交的：事务可以看到其他事务操作但未提交的结果-脏读 开发中不允许出现

2.read committed-读已提交的：事务可以看到其他事务提交之后的结果<不可重复读>

3.repeatable read-可重复读<默认级别>：a事务提交数据之后事务b读不到-幻读

4.serializable-串行化<性能差>：数据表被某个事务操作时，其他事务里的写入操作不可以进行，进入排队状态

性能比较<隔离级别越高，性能越差>：read uncommitted>read committed>repeatable read>serializable
