Oracle 10G数据库有3种类型的特权：

1. 对象特权模式对象上的特权，比如表、视图、序列、过程和包等模式对象。要想使用这些对象，需要该对象上的特权。

2. 系统特权 数据库级操作上的特权，比如连接到数据库，创建用户、更改数据库或消耗极大数据的表空间等操作。

3. 角色特权一个用户作为一个角色所拥有的对象与系统特权。角色是用来管理特权组的工具。

 

**序列**

1. CREATE SEQUENCE允许被授权者在他们自己的模式中创建新的序列

2. CREATE ANY SEQUENCE允许被授权者在任意一个模式中创建新的序列

3. ALTER ANY SEQUENCE允许被授权者修改数据库中任意一个序列的属性

4. DROP ANY SEQUENCE允许从数据库内的任意一个模式中删除任意一个序列

5. SELECT ANY SEQUENCE

 

**会话**

1. CREATE SESSION允许被授权者连接到数据库。该特权对用户账户是必需的，但对软件账户可能是不受欢迎的。

2. ALTER SESSION允许被授权者执行ALTER SESSIONS语句

3. ALTER RESOURCE COST允许被授权者修改ORACLE为一个概况中的资源约束计算资源成本的方式。

4. RESTRICTED SESSION允许数据库在RESTRICTED SESSION模式时连接到数据库，一般是为了管理性目的。

 

**表空间**

1. CREATE TABLESPACE允许创建新的表空间

2. ALTER TABLESPACE允许被授权者更改现有表空间

3. DROP TABLESPACE允许删除表空间

4. MANAGE TABLESPACE允许更改表空间。例如ONLINE、OFFILE、BEGIN BACKUP或END BACKUP

5. UNLIMITED TABLESPACE允许消耗任意一个表空间中的磁盘限额。相当于给指定授权者每个表空间中的无限限额。以上介绍Oracle系统特权。

 

**表**

1. CREATE TABLE允许在自己的对象模式中创建表

2. CREATE ANY TABLE允许在任意一个对象模式中创建表

3. ALTER ANY TABLE允许更改任意一个对象模式中的表

4. DROP ANY TABLE允许从任意一个对象模式中删除表

5. COMMENT ANY TABLE允许给任意一个对象模式中的任意一个表或列注释

6. SELECT ANY TABLE允许查询任意表

7. INSERT ANY TABLE允许插入新行到任意表

8. UPDATE ANY TABLE允许更新任意表

9. DELETE ANY TABLE允许删除任意表中的行

10. LOCK ANY TABLE允许执行一条LOCK TABLE来明确锁定任意一个表

11. FLASHBACK ANY TABLE允许使用AS OF 语法对任意一个对象模式的任意一个表或视图执行一个SQL回闪查询。

 

**同义词**

1. CREATE SYNONYM允许在自己的对象模式中创建同义词

2. CREATE ANY SYNONYM允许在任意对象模式中创建新的同义词

3. CREATE PUBLIC SYNONYM允许被授权者创建新的公用同义词。这些同义词对数据库中的所有用户都是可访问的。

4. DROP ANY SYNONYM允许从任意对象模式中删除任意一个同义词

5. DROP PUBLIC SYNONYM允许被授权者从数据库中删除任意一个公用同义词

 

**表对象特权**

1. SELECT允许查询指定表

2. INSERT允许在指定表创建新行

3. UPDATE允许修改指定表的现有行

4. DELETE允许删除指定表的行
5. ALTER允许添加、修改或重命名指定表中的列，转移该表到另一个表空间，乃至重命名指定表。

6. DEBUG允许被授权者借助于一个调度程序访问指定表上的任意触发器中的PL/SQL代码

7. INDEX允许被授权者在指定表上创建新的索引

8. REFERENCES允许创建参考指定表的外部键约束

**视图对象特权**

1. SELECT查询指定视图

2. INSERT允许在指定视图创建新行

3. UPDATE允许修改指定视图的现有行

4. DELETE允许删除指定视图的行

5. DEBUG允许被授权者借助于一个调度程序访问指定视图上的任意触发器中的PL/SQL代码

6. REFERENCES允许创建参考指定视图的外部键约束

**序列对象特权**

1. SELECT允许访问当前值和下一个值(即CURRVAL和NEXTVAL)

2. ALTER允许修改指定序列的属性

 

 