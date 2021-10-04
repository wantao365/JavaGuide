> **Oracle系统默认的几个用户：**
>
> ​    sys --------本地管理用户，具有最高数据库管理权限
>
> ​    system------网络管理用户，权限次于sys
>
> ​    scott-------普通用户，默认是锁住的（不可用）
>
> ​    默认情况下：
>
> ​       scott 密码是 tiger                       sys 密码是 change_on_install 
>
> ​       system 密码是 manager            sysman 密码是 oem_temp
>
>  
>
> **启动监听器服务：lsnrctl start**
>
> **启动实例服务：oradim -starup -sid orcl**
>
> **显示当前用户**：show user;
>
> **登录用户**：sqlplus / as sysdba (sys网络管理员用户登录)
>
> ​        sqlplus username/password (普通用户)
>
> ​    -->sqlplus
>
> ​    -->请输入用户名：username
>
> ​    -->请输入口令：password
>
>  
>
> **创建用户**：create user zhangsan identified by zhangsan;
>
> **修改用户密码**：alter user lisi identified by lll;
>
>  
>
> **用户断开数据库连接**：disconn;
>
> **当前用户重新连接**：conn username/password;
>
> 删除用户：drop user username;
>
> 设置显示宽度：set linesize 400;
>
>  
>
> 系统权限管理：
>
> ​    授予会话权限：grant create session to zhangsan;
>
> ​    授予建表权限：grant create table to zhangsan;
>
> ​    授予无限制使用表空间的权限：grant unlimited tablespace to zhangsan;
>
> ​    授予权限：grant 权限 to 用户名;   
>
> ​    撤销权限：revoke 权限 from 用户名;
>
> 查询用户的系统权限：select * from user_sys_privs;
>
>  
>
> 用户权限管理：
>
> ​    grant select on mytab to lisi;   
>
> ​    grant update on mytab to lisi;
>
> ​    grant delete on mytab to lisi;
>
> ​    grant insert on mytab to lisi;
>
> ​    revoke select on mytab from lisi;
>
> ​    授予其他用户对当前用户表中的【某个字段】的操作权限：
>
> ​       grant update(pass) on mytab to lisi;
>
> ​    授予其他用户操作表的所有权限：
>
> ​       grant all on mytab to lisi;
>
> ​    撤销其他用户操作表的所有权限：
>
> ​       revoke all on mytab from lisi;
>
> 查询其他用户对【当前用户表】的操作权限：
>
> ​    select * from user_tab_privs;
>
> 查询其他用户对【当前用户表字段】的操作权限：
>
> ​    select * from user_col_privs;
>
>  
>
> 权限传递：
>
> ​    系统权限：grant create session to zhangsan with admin option;
>
> ​    (表示把系统权限授予给zhangsan，并允许其授予给其他用户)
>
> ​    用户权限：grant update on mytab to lisi with grant option;
>
> ​    (表示把用户权限授予给lisi，并允许其授予给其他用户)
>
>  
>
> 角色管理：
>
> ​  创建角色：create role roleName;
>
> ​  给角色授予权限：grant 权限 to roleName;
>
> ​  将角色授予给用户：grant roleName to userName;
>
> ​  用户查询拥有的角色：select * from user_role_privs;
>
> ​  删除角色：drop role roleName;
>
> ​  当给角色授予权限的时候，拥有此角色的用户也同时增加了权限；
>
> ​  当撤销角色权限的时候，拥有此角色的用户的对应权限也被撤销；
>
> ​  当角色被删除，拥有此角色的用户将丧失之前角色所有的所有权限。
>
>  
>
>   修改表结构：alter table mytab add pass varchar(20);