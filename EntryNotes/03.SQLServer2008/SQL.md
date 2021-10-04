# DDL(数据库)

```sql
--SQL指令编辑窗口:在此可以输入SQL指令完成对数据库的操作

--有关数据库操作的DDL指令

--（SQLServer数据库是一个文件系统的数据库，也就是说，我们将数据存储在数据库中实际上是存在数据库的某个文件中）
--一个SQLServer数据库包含一个数据文件，和一个日志文件
--数据文件(mdf):存储数据库中的数据
--日志文件(ldf):存储数据库的操作日志

--①创建一个新的数据库
create database db_test
on(
	name='db_test',                       --指定数据文件逻辑名称
	filename='c:\db_files\test.mdf',      --指定数据文件的物理路径
	size=10,							  --指定数据文件初始大小
	maxsize=100,                          --指定数据文件最大容量
	filegrowth=5						  --指定数据文件增量
)log on(
	name='db_test_log',
	filename='c:\db_files\testlog.ldf',
	size=2,
	maxsize=10,
	filegrowth=1
);


--drop:删除数据库对象
--delete:删除数据表中的数据
drop database db_test;


--进入/使用指定数据库
use db_test;

--数据库的备份与恢复

--备份
backup database db_test to disk='c:\code\aaa.dat' with format,name='test';

--查看备份文件内部信息
restore filelistonly from disk='c:\code\aaa.dat';

--恢复
restore database db_test from disk='c:\code\aaa.dat'
with move 'db_test' to 'c:\db_files\test.mdf',
move 'db_test_log' to 'c:\db_files\testlog.ldf';
```

# DDL(数据表)

```sql
--1.创建数据表
--一个数据库中可以包含多个数据表，数据是存储在数据表中的
create table tb_students(
	stu_num int primary key identity(1000001,1),
	stu_name varchar(20) not null,
	stu_sex char(2) not null check(stu_sex='男' or stu_sex='女') default('男'),
	stu_age int not null check(stu_age>0 and stu_age<100),
	stu_tel char(11) unique
);

create table tb_courses(
	course_num int primary key identity(1001,1),
	course_name varchar(50) not null,
	course_xf int not null
);

create table tb_grade(
  snum int,
  cnum int,
  score int not null,
  primary key(snum,cnum), --联合主键的设置
  constraint fk_grade_student foreign key(snum) references tb_students(stu_num),
  constraint fk_grade_course foreign key(cnum) references tb_courses(course_num)
);

insert into tb_students(stu_name,stu_age,stu_tel) values('张三',22,'13030303301');
insert into tb_students(stu_name,stu_age,stu_tel) values('李四',23,'13030303302');
insert into tb_courses(course_name,course_xf) values('语文',3);
insert into tb_courses(course_name,course_xf) values('数学',4);

insert into tb_grade values(1000001,1002,95);

select * from tb_students;
select * from tb_courses;

select * from tb_grade;

--SQL中的数据类型：
--整型：int
--浮点：float,decimal(6,2)
--字符：char定长字符串,varchar可变长度字符串

--2.删除数据表(如果当前表被其他表的外键关联，则需先删除其他的表)
drop table tb_grade;
drop table tb_students;
drop table tb_courses;


--3.数据表中的字段约束
--字段不为空：not null
--字段值唯一：unique
--字段值类型：char/varchar/int/float...`
--字段值长度：字段类型后在()中定义
--主键约束：primary key（默认就unique）
--外键约束
--check约束
--默认值
--自动增长：字段必须是整型 course_num int primary key identity(1001,1)

--4.修改数据表

--alter/drop:对数据库中的对象进行修改/删除操作
--update/delete:对数据表中的数据进行修改/删除操作

--添加字段
alter table tb_students add stu_addr varchar(100);
--修改字段约束
alter table tb_students alter column stu_addr varchar(50) not null;

--修改表中的主外键关系
alter table tb_grade alter column snum int not null;
alter table tb_grade alter column cnum int not null;

--添加/删除主键
alter table tb_grade add constraint PK_GRADE primary key(snum,cnum);
alter table tb_grade drop constraint PK_GRADE;
--添加/删除外键
alter table tb_grade add constraint FK_GRADE_STUDENTS foreign key(snum) references tb_students(stu_num);
alter table tb_grade add constraint FK_GRADE_COURSES foreign key(cnum) references tb_courses(course_num);
alter table tb_grade drop constraint FK_GRADE_STUDENTS;
alter table tb_grade drop constraint FK_GRADE_COURSES;


select * from tb_students;
select * from tb_coures;
```

# DML(增删改)

```sql
---------------------------------------------------------------------------
--删除数据库
drop database db_test;
--创建数据库
create database db_test
on(
	name='db_test',
	filename='c:\db_files\test.mdf',
	size=10,
	maxsize=100,
	filegrowth=2
)log on(
	name='db_test_log',
	filename='c:\db_files\testlog.ldf',
	size=2,
	maxsize=10,
	filegrowth=1
);
--进入数据库
use db_test;

--创建数据表
create table tb_stus(
	stu_num char(5) primary key,
	stu_name varchar(20) not null,
	stu_sex char(2) check(stu_sex='男' or stu_sex='女') default('男'),
	stu_age int,
	stu_tel char(11) unique
);

create table tb_courses(
	course_num int primary key identity(1001,1),
	course_name varchar(50) not null,
	course_xf int not null
);

create table tb_grade(
	snum char(5) not null,
	cnum int not null,
	score int not null,
	primary key(snum,cnum),
	constraint FK_GRADE_STUS foreign key(snum) references tb_stus(stu_num),
	constraint FK_GRADE_COURSE foreign key(cnum) references tb_courses(course_num)
);

--我们可以在数据表中存储数据

--增：向数据表中添加一条记录(一个元组)
--insert into <tableName>[(colnum1,column2,column3...)] values(v1,v2,v3...);
insert into tb_stus values('10010','张三','男',23,'13030303300');
insert into tb_stus(stu_num,stu_name,stu_sex,stu_tel) values('10011','李四','女','13030303301');

--要求：当进行添加操作时，必须在表名后通过()列出赋值的字段名，即使是所有字段，也请列出字段名

insert into tb_stus values('10012','王武','男',24,'13030303302');
insert into tb_stus(stu_num,stu_name,stu_sex,stu_age,stu_tel) values('10013','赵柳','女',25,'13030303303');
--好处①：所给的值无需和表定义的列顺序保持一致，只需和表名后列出的字段名保持一致
insert into tb_stus(stu_num,stu_sex,stu_name,stu_age,stu_tel) values('10014','男','二狗子',25,'13030303303');

--好处②：可以提交语句的兼容性/健壮性
alter table tb_stus add stu_addr varchar(200);
insert into tb_stus values('10015','孙七','男',21,'13030303304');
insert into tb_stus(stu_num,stu_sex,stu_name,stu_age,stu_tel) values('10016','女','娃哈哈',25,'13030303305');



--删：从数据表中删除不再需要的数据（删除操作针对一个或者多个元组/记录）
--delete from <tableName> [where conditions];
--如果delete语句后没有where子句，则表示删除当前表中所有数据
--如果有where子句，则表示删除满足where子句条件的记录
delete from tb_stus where stu_num='10016';
delete from tb_stus where stu_age<24 or stu_age>25;
delete from tb_stus where stu_age between 24 and 26; --包含24和26
delete from tb_stus where stu_age not between 24 and 26;
delete from tb_stus;


--改：修改数据表中某条记录的某一列或者某几列
update tb_stus set stu_age=22 where stu_num='10011';
update tb_stus set stu_name='高松柏',stu_age=19 where stu_num='10016';
update tb_stus set stu_name='小花',stu_sex='女',stu_age=17,stu_tel='13131313311' where stu_num='10015';
update tb_stus set stu_age=stu_age+1 where stu_sex='女';
```

# 单表查询语句

```sql
--创建数据表
create table tb_stus(
	stu_num char(5) primary key,
	stu_name varchar(20) not null,
	stu_sex char(2) check(stu_sex='男' or stu_sex='女') default('男'),
	stu_age int,
	stu_tel char(11) unique
);

create table tb_courses(
	course_num int primary key identity(1001,1),
	course_name varchar(50) not null,
	course_xf int not null
);

create table tb_grade(
	snum char(5) not null,
	cnum int not null,
	score int not null,
	primary key(snum,cnum),
	constraint FK_GRADE_STUS foreign key(snum) references tb_stus(stu_num),
	constraint FK_GRADE_COURSE foreign key(cnum) references tb_courses(course_num)
);

--查询：获取数据库中的数据，并显示出来（不会影响数据库中的记录）

--单表查询：从某一张表中查询数据

--"*"表示查询出满足条件的记录的所有列
select * from tb_stus;

--显示制定列
select stu_num,stu_name,stu_age from tb_stus;

--给字段名取别名
select stu_num as '学号',stu_name '姓名',stu_age '年龄' from tb_stus;

--计算列
select stu_num,stu_name,2016-stu_age '出生年份' from tb_stus;

--条件查询：在select语句后添加where子句，表示查询满足条件的数据
select * from tb_stus where stu_sex='女';
select * from tb_stus where stu_age>20;

--like关键字：
--查询出名字中包含字母"T"的学生信息（%表示0~n个字符）
select * from tb_stus where stu_name like '%T%';
--查询出名字中首字母为"T"的学生信息
select * from tb_stus where stu_name like 'T%';
--查询出名字中第二个字母为“i”的学生信息(_表示任意一个字符)
select * from tb_stus where stu_name like '_i%';

--between..and..关键字：闭区间
select * from tb_stus where stu_age between 20 and 24;

--not关键字:
--查询出名字中首字母不为"T"的学生信息
select * from tb_stus where stu_name not like 'T%';
select * from tb_stus where stu_age not between 20 and 24;

--多条件查询（or,and关键字）：
select * from tb_stus where stu_age>20 and stu_sex='女'; --&&
select * from tb_stus where stu_age<=20 or stu_sex='女'; --||



--count(*):统计记录数
--avg(column):统计指定字段的平均值
--max(column):获取当前字段的最大值
--min(column):获取当前字段的最小值
--sum(column):统计指定字段的值的和
select COUNT(*) from tb_stus where stu_age>20;
select AVG(stu_age) from tb_stus group by stu_sex ;

--group by 分组查询：select后只能显示被分组的字段名、聚合函数
select stu_sex,COUNT(*),AVG(stu_age),MAX(stu_age),MIN(stu_age),SUM(stu_age) from tb_stus group by stu_sex;

--在group by分组查询中，where可以对分组前的数据进行条件筛选
select stu_sex,COUNT(*) '人数' from tb_stus where stu_age>20 group by stu_sex;

--在group by分组查询中，having可以对分组后的数据进行条件筛选
select stu_sex,COUNT(*) '人数' from tb_stus group by stu_sex having COUNT(*)>3;

--嵌套查询：
select * from tb_stus where stu_age>20 and stu_num in (select stu_num from tb_stus where stu_sex='女');

--显示查询出的前5条记录
select top(5) * from tb_stus;

--显示查询出的前5条记录的学号
select top(5) stu_num from tb_stus;

--查询除了前五条学生以外的其他的所有学生
select top(5) * from tb_stus where stu_num not in (select top(0*5) stu_num from tb_stus);  --第一个五条
select top(5) * from tb_stus where stu_num not in (select top(1*5) stu_num from tb_stus);  --第二个五条
select top(5) * from tb_stus where stu_num not in (select top(2*5) stu_num from tb_stus);  --第三个五条

--无条件分页查询：每页显示m条，查询第n页
--select top(m) * from tb_stus where stu_num not in (select top(m*(n-1)) stu_num from tb_stus); --第n个五条

--order by:对查询结果按照某个字段进行排序（asc 升序；desc 降序）
select * from tb_stus order by stu_age asc;
select * from tb_stus order by stu_age desc;
--多字段排序
select * from tb_stus order by stu_sex desc,stu_age asc;

insert into tb_stus values('10014','Polly','男',4,'13232323300');
insert into tb_stus values('10017','haha','女',5,'13232323301');
insert into tb_stus values('10018','hehe','男',6,'13232323302');
insert into tb_stus values('10019','xixi','女',7,'13232323303');
insert into tb_stus values('10020','heihei','男',8,'13232323304');


select * from tb_stus where stu_age>20;

select * from tb_stus;
select * from tb_grade;
select * from tb_courses;

insert into tb_courses(course_name,course_xf) values('Java',3);
insert into tb_courses(course_name,course_xf) values('C++',4);
insert into tb_courses(course_name,course_xf) values('C#',2);
insert into tb_courses(course_name,course_xf) values('PHP',1);

insert into tb_grade(snum,cnum,score) values(10010,1001,96);
insert into tb_grade(snum,cnum,score) values(10010,1003,99);

insert into tb_grade(snum,cnum,score) values(10011,1001,88);
insert into tb_grade(snum,cnum,score) values(10011,1002,87);
insert into tb_grade(snum,cnum,score) values(10011,1003,86);

insert into tb_grade(snum,cnum,score) values(10014,1001,89);
insert into tb_grade(snum,cnum,score) values(10014,1002,91);
insert into tb_grade(snum,cnum,score) values(10014,1003,99);

--1.查询出参加考试的同学信息
select * from tb_stus where stu_num in (select distinct snum from tb_grade);
select * from tb_stus where stu_num not in (select distinct snum from tb_grade);

--2.查询出被学生选择的课程信息
select * from tb_courses where course_num in(select distinct cnum from tb_grade);

--3.查询出选了2门课以上的学生信息
select snum from tb_grade group by snum having COUNT(*)>2;
select * from tb_stus where stu_num in(select snum from tb_grade group by snum having COUNT(*)>2);

--4.查询参加考试的学生的姓名，课程名称及这门课的分数
select s.stu_name,c.course_name,g.score from tb_stus s,tb_courses c,tb_grade g where s.stu_num=g.snum and g.cnum=c.course_num;
```

# 多表联合查询

```sql
--创建部门表
create table dept(
	deptno int primary key identity(1001,1),
	dname varchar(20) not null,
	daddr varchar(30) not null
);

--创建员工表
create table emp(
	eno varchar(5) primary key,
	name varchar(20) not null,
	age int not null,
	sal decimal(8,2) not null,
	deptno int,
	constraint FK_EMP_DEPT foreign key(deptno) references dept(deptno)
);

--添加5个部门信息
insert into dept(dname,daddr) values('市场部','武汉');
insert into dept(dname,daddr) values('研发部','宜昌');
insert into dept(dname,daddr) values('人事部','北京');
insert into dept(dname,daddr) values('财务部','上海');
insert into dept(dname,daddr) values('售后服务部','深圳');

--添加8个员工信息
--研发部（1002）
insert into emp(eno,name,age,sal,deptno) values('10001','张三',23,1000.00,1002);
insert into emp(eno,name,age,sal,deptno) values('10003','李四',29,1300.00,1002);
insert into emp(eno,name,age,sal,deptno) values('10007','赵柳',33,1700.00,1002);

--市场部（1001）
insert into emp(eno,name,age,sal,deptno) values('10002','高松柏',19,800.00,1001);
insert into emp(eno,name,age,sal,deptno) values('10004','罗环宇',21,1200.00,1001);

--财务部（1004）
insert into emp(eno,name,age,sal,deptno) values('10005','夏晟阁',22,3000.00,1004);

--无部门
insert into emp(eno,name,age,sal,deptno) values('10006','石兴顺',26,2400.00,null);
insert into emp(eno,name,age,sal,deptno) values('10008','二狗子',28,2000.00,null);


--多表联合查询：查询的数据来自于多张表
select * from dept;
select * from emp;

--全组合排列
select * from dept,emp;

--等值连接查询：列出两张表中某个字段值相等的匹配
select * from dept,emp where dept.deptno = emp.deptno;

--自然连接查询：是一种特殊的等值连接
--1.无需指定字段，可以自动匹配
--2.查询结果要去除重复列
select dept.deptno,dname,daddr,eno,name,age,sal from dept,emp where dept.deptno = emp.deptno;



--左连接：左表中的数据都会显示
select * from dept left join emp on dept.deptno=emp.deptno; 

--右链接：右表中的数据都会显示
select * from dept right join emp on dept.deptno=emp.deptno; 

--全连接：两个表中的数据都会先
select * from dept full join emp on dept.deptno=emp.deptno;

--等值连接：只显示两张表中能够匹配的记录
select * from dept join emp on dept.deptno=emp.deptno;
select * from dept,emp where dept.deptno = emp.deptno;
--给表取别名
select * from dept d,emp e where d.deptno = e.deptno;


--查询"研发部"的部门编号
select deptno from dept where dname='研发部';
--查询出员工表中部门编号为1002的员工信息
select * from emp where deptno=1002;
--查询出研发部所有员工信息
select * from emp where deptno in (select deptno from dept where dname='研发部');

create view myView as (select deptno,COUNT(*) 'num' from emp group by deptno having deptno>0);


select * from dept left join myView v on dept.deptno=v.deptno;
--查询有员工的部门名称
select dname from dept where deptno in(select deptno from emp);

--distinct：去除查询结果的重复项
select distinct deptno from emp;

--集合查询：将多条查询语句的结果集进行合并
--要求：两个查询语句的结果集字段数目和类型一致
select deptno,dname from dept UNION select eno,name from emp;


--视图：可以将查询的结果单独创建成一张“表”--视图
--create view <viewName> as select...;
--对视图的更新(删除/添加/修改)都会影响到原数据表

--创建视图
create view stu_nv as (select * from tb_stus where stu_sex='女');
--查询视图
select * from stu_nv;
--删除视图中的数据(会从原表中删除)
delete from stu_nv where stu_num='10015';
--向视图中添加数据（会添加到原表）
insert into stu_nv values('10015','Lucy','女',22,'13031323334');
--删除视图（删除数据表，会连同数据表中的数据一同删除；但是删除视图不会影响数据）
drop view stu_nv;

select * from tb_stus;
```

# 系统存储过程

```sql
--常用的系统存储过程

--列出当前数据库服务器上所有的数据库
execute sp_databases;

--列出所有/指定数据库的详细信息
exec sp_helpdb;
exec sp_helpdb 'db_test';

--修改数据库名称
--第一个参数：原数据库名
--第二个参数：新数据库名
exec sp_renamedb 'db_hehe','db_test';

--列出当前数据库中所有的表和视图信息
exec sp_tables;

--列出指定数据表的字段信息
exec sp_columns 'tb_stus';

--列出当前数据库下所有的数据库对象（对象名称，所属者，类型）
exec sp_help;
--列出指定的数据表的详细信息（表信息，字段信息，索引，视图，约束，主外键）
exec sp_help 'tb_stus';

--查看指定表的主键、外键的约束信息
exec sp_helpconstraint 'tb_grade';

--查看指定表中的索引信息
exec sp_helpindex 'tb_stus';

--列出当前数据库下所有的存储过程
exec sp_stored_procedures;

--修改当前账户的密码
exec sp_password 'oldPwd','newPwd','sa';
```

# 存储过程

```sql
--定义存储过程
create proc proc_sum
    @a int,
    @b int,
    @c int output
as
    set @c = @a+@b;
go



--调用存储过程
begin
   declare @k int;
   exec proc_sum 9,4,@k out;
   print(@k);
end;


--无输入参数无输出参数
--创建一个存储过程，查询学生表中所有信息,并打印学生总数和学生平均年龄
create procedure proc_test1
as
	select * from tb_stus;
	declare @num int;
	select @num=COUNT(*) from tb_stus;
	print('学生总数为：'+convert(varchar,@num));
	declare @age int;
	select @age=AVG(stu_age) from tb_stus;
	print('平均年龄为：'+convert(varchar,@age));
go

drop procedure proc_test1;

exec proc_test1;

--有输入参数无输出参数
--创建一个存储过程，查询指定年龄段的学生信息
create procedure proc_test2
	@min_age int,
	@max_age int
as
	select * from tb_stus where stu_age between @min_age and @max_age;
go

--直接传参
exec proc_test2 18,27;

--通过变量传参
begin
   declare @i int =20;
   declare @j int =26;
   exec proc_test2 @i,@j;
end;

--无输入参数有输出参数
--创建一个存储过程，分别统计男生和女生的数量，以输出参数返回
create procedure proc_test3
	@num1 int out,
	@num2 int output
as
	select @num1=COUNT(*) from tb_stus where stu_sex='男';
	select @num2=COUNT(*) from tb_stus where stu_sex='女';
go

--调用带有输出参数的存储过程，需要定义变量接值
begin
	declare @m1 int;
	declare @m2 int;
	exec proc_test3 @m1 out,@m2 out;
	print('男生人数为:'+convert(varchar,@m1));
	print('女生人数为:'+convert(varchar,@m2));
end;

--有输入参数和输出参数
--创建一个存储过程，输入一个学生的学号
--和课程编号，输出学生的姓名，课程名称和成绩
create procedure proc_test4
	@snum char(5),
	@cnum int,
	@sname varchar(20) out,
	@cname varchar(50) out,
	@score int=0 out
as
	select @sname=stu_name from tb_stus where stu_num=@snum;
	select @cname=course_name from tb_courses where course_num=@cnum;
	select @score=score from tb_grade where snum=@snum and cnum=@cnum;
go

--调用有输入输出参数的存储过程
begin
	declare @fs int;
	declare @sn varchar(20);
	declare @cn varchar(50);
	exec proc_test4 '10011',1003,@sn out,@cn out,@fs out;
	print(@sn+' 的 '+@cn+' 考试成绩为:'+convert(varchar,@fs));
end;

select * from tb_stus

--存储过程练习：
--1.创建一个存储过程，根据输入的学号，查询这个学生是否参加考试
--如果参加考试则打印出学生姓名-课程名称-成绩
--如果没有参加考试，则提示“此学生未参加考试”。
create procedure proc_test5
	@snum char(5)
as
	declare @c int;
	select @c=COUNT(*) from tb_grade where snum=@snum;
	if(@c>0)begin
		select s.stu_name,c.course_name,g.score from tb_stus s,tb_courses c,tb_grade g
		where s.stu_num=g.snum and g.cnum=c.course_num and g.snum=@snum;
	end else begin
		print('此学生未参加考试！');
	end;
go


exec proc_test5 '10011'; 

--创建一个存储过程,对学生表进行分页
--输入参数：每页显示的条数pageSize（m），页码pageNum（n）
--输出参数：总页数
--查询出对应页码的结果集
create procedure proc_stu_splitpage
	@pageSize int, 
	@pageNum int,
	@pageCount int output	
as
	--查询到总记录数
	declare @recordCount int;
	select @recordCount=COUNT(*) from tb_stus;
	--计算总页数
	if( @recordCount%@pageSize=0 )begin
		set @pageCount = @recordCount/@pageSize;
	end else begin
		set @pageCount = @recordCount/@pageSize+1;
	end
	--分页查询
	select top(@pageSize) * from tb_stus where stu_num not in
	(select top((@pageNum-1)*@pageSize) stu_num from tb_stus);
go

begin
	declare @pc int;
	exec proc_stu_splitpage 4,2,@pc out;
	print('总页数为:'+convert(varchar,@pc));
end;



--女生
select top(3) * from tb_stus where stu_sex='女';

--不带条件的分页：
--select top(@pageSize) * from tableName where PK not in ( select top(@pageSize*(@pageNum-1)) PK from tableName);

--带条件的分页
--select top(@pageSize) * from tableName where PK not in ( select top(@pageSize*(@pageNum-1)) PK from tableName where conditions) and conditions;

select top(3) * from tb_stus where stu_num not in( select top(3) stu_num from tb_stus where stu_sex='女' ) and stu_sex='女';


select top(3) * from tb_stus where stu_num not in( select top(3) stu_num from tb_stus );


--创建一个存储过程条件学生信息
create procedure proc_save_student
	@num char(5),
	@name varchar(20),
	@sex char(2),
	@age int,
	@tel char(11)
as
	if(@age>100 or @age<0)begin
		raiserror('年龄不合法',18,1);
	end else begin
		insert into tb_stus(stu_num,stu_name,stu_sex,stu_age,stu_tel) values(@num,@name,@sex,@age,@tel);
	end;
go

exec proc_save_student '10018','Hehe','男',9,'13536373839';
```

# 分页存储过程

```sql
--创建一个存储过程，完成对tb_stus的分页

--分页存储过程：就是对表中的数据进行分页查询

--从学生表中查询出第n页，每页显示m条
--1.先查询出前2页的记录的学号
--select top(m*(n-1)) stu_num from tb_stus;
--2.再查询出除了这两页以外的所有的学生
--select * from tb_stus where stu_num not in( select top(m*(n-1)) stu_num from tb_stus );
--3.从剩下的所有学生信息取出前20条
--select top(m) * from tb_stus where stu_num not in( select top(m*(n-1)) stu_num from tb_stus );
--select top(m) * from tb_stus where stu_num not in( select top(m*(n-1)) stu_num from tb_stus where conditions) and conditions;
select top(20) * from tb_stus where stu_num not in( select top(20*(3-1)) stu_num from tb_stus where stu_sex='女' ) and stu_sex='女';


create proc proc_splitpage
	@pageSize int,
	@pageNum int,
	@pageCount int output
as
	--1.查询出学生的总数
	declare @count int;
	select @count=COUNT(*) from tb_stus;
	--2.计算总页数
	if(@count%@pageSize=0)begin
		set @pageCount = @count/@pageSize;
	end else begin
		set @pageCount = @count/@pageSize + 1;
	end;
	--3.进行分页
	select top(@pageSize) * from tb_stus where stu_num not in
	( select top(@pageSize*(@pageNum-1)) stu_num from tb_stus );
go



--如果我要对emp表分页？

create proc proc_splitpage
	@tableName varchar(50),		--被分页的表名 ‘emp’
	@pkName varchar(30),		--被分页的表的主键名 ‘eno’
	@columns varchar(100)='*',	--显示字段名列表
	@condition varchar(100)='', --分页条件
	@orderStr varchar(100)='',  --排序规则
	@pageSize int,				--每页显示的记录数
	@pageNum int,				--显示目标页
	@pageCount int output		--返回总页数
as
	--1.查询总记录数
	declare @count int;
	declare @sql1 nvarchar(200);
	set @sql1 = 'select @a=COUNT(*) from '+@tableName;
	exec sp_executesql @sql1,N'@a int output',@count output;
	
	--2.计算总页数
	if(@count%@pageSize=0)begin
		set @pageCount = @count/@pageSize;
	end else begin
		set @pageCount = @count/@pageSize + 1;
	end;
	
	--3.进行分页
	declare @sql2 nvarchar(100);
	set @sql2 = 'select top('+convert(varchar,@pageSize*(@pageNum-1))+') '+@pkName+' from '+@tableName;
	--拼接分页条件
	if(@condition!='')begin
		set @sql2 = @sql2 + ' where '+@condition;
	end;
	--拼接排序规则
	if(@orderStr!='')begin
		set @sql2 = @sql2 + ' order by '+@orderStr;
	end;
	
	declare @sql3 nvarchar(200);
	set @sql3 = 'select top('+convert(varchar,@pageSize)+') '+@columns+' from '+@tableName+' where '+@pkName+' not in('+@sql2+')';
	--拼接分页条件
	if(@condition!='')begin
		set @sql3 = @sql3 + ' and '+@condition;
	end;
	--拼接排序规则
	if(@orderStr!='')begin
		set @sql3 = @sql3 + ' order by '+@orderStr;
	end;
	
	exec(@sql3);
go




begin
	declare @pc int;
	exec proc_splitpage 'tb_stus','stu_num','stu_age>20','stu_age desc','stu_name,stu_sex,stu_age',3,2,@pc out;
end;






drop proc proc_test6;

create proc proc_test6
	@tableName varchar(50)
as
	declare @sql varchar(200);
	set @sql = 'select * from '+@tableName;
	--如何执行一个字符串类型的SQL指令
	exec(@sql);
go


begin
	exec  proc_test6 'dept';
end;
```

# 触发器

```sql
--创建触发器
create trigger tri_test on tb_stus for insert
as
    print('又添加一个学生信息');
go

--修改触发器
alter trigger tri_test2 on tb_stus for delete
as
	select * from tb_stus;
go

--删除触发器
drop trigger tri_test;

insert into tb_stus(stu_num,stu_name,stu_sex,stu_age,stu_tel)
values('10021','XiaoZhang','男',39,'13434343347');

delete from tb_stus where stu_num='10020';


select * from tb_stus;
```

# JDBC调用存储过程

```sql
--只有输入参数的存储过程
create procedure proc_test1
	@minage int,
	@maxage int
as
	select * from emp where age between @minage and @maxage;
go

drop proc proc_test1;

--调用只有输入参数的存储过程
exec proc_test1 20,30;

--没有参数，有多个结果集的存储过程
create proc proc_test2
as
	select * from dept;
	select * from emp;
go

exec proc_test2;


--带有输入参数、输出参数和多个结果集的存储过程
create proc proc_test3
	@age int,
	@keyWord varchar(2),
	@name varchar(20) output,
	@avgAge int output
as
	select * from emp where age<@age;
	select * from emp where name like '%'+@keyWord+'%';
	select @name=name from emp where age in(select max(age) from emp);
	select @avgAge=avg(age) from emp;
go

begin
	declare @a varchar(20);
	declare @m int;
	exec  proc_test3 20,'娜',@a output,@m output;
	print(@a);
	print(@m)
end;
```

