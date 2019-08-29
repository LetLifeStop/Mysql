## Mysql

一个服务器下有多个库，一个库下有多个表，一个表下有多行多列的数据



关系型数据库存储在磁盘中；非关系型数据库存储在内存中

关系模型由 **关系数据结构**，**关系操作集合**，**关系完整性约束** 三部分构成

**关系数据结构**：指 数据以什么方式来存储，是一种**二维表**的存储

**关系操作集合**：如果来关联和管理对应的存储数据，SQL指令

**关系完整性约束**：数据内部有对应的关联关系，以及数据与数据之间也有对应的关联关系



典型关系型数据库：

小型关系型数据库：Microsoft Access ，SQLite

中型关系型数据库：SQL Server ，Mysql

大型关系型数据库：Oracle，DB2



### SQL

结构化查询语言（structured Query Language），特殊目的的编程语言

**分类：**

1. 数据查询语言（DQL，Data Query Language） 查询数据
2. 数据操作语言（DML，Data Manupulation language）增加，删除，修改数据
3. 事务处理语言（TPL，能保证DML语句影响的表的所有行及时得到更新）用于事务安全处理
4. 数据控制语言（DCL）用于权限管理
5. 数据定义语言（DDL） 在数据库中创建新表或删除表，用于结构管理

统称为SQL



#### 启动和停止Mysql服务

C/S 模型

服务器端对应的软件：Mysqld.exe

**命令行方式：**

cmd控制台

具体操作方法：

1. net start mysql ,这个是启动mysql服务器
2. mysql -u root -p 登录
   * -h 指定客户端所要登录的Mysql主机名，可省略，默认是local post
   * -u 登录的用户名
   * -p通过密码登录
   * -P 端口号
3. 然后就能进行操作了
4. net stop mysql 这个是关闭mysql服务器

**系统方式：**

1. 我的电脑右键，管理
2. 服务，找到mysql
3. 双击，然后点击启动



#### Mysql服务端架构

如下几层：

1. 数据库管理系统（最外层）：DBMS，专门管理服务器端所有的数据
2. 数据库（第二层）：DB，存储数据的仓库
3. 二维数据表（第三层） ：Table，存储具体的数据
4. 字段（第四层）：field，具体存储某种类型的数据（实际存储单元）

关键字：

Row：行 

column：列 

#### 数据库基本操作

只要使用 数据库.表名 这个格式，就可以在任何数据库下访问其他数据库的表名

**创建数据库：**

```mysql
-- 创建数据库
create database mydatabase；
-- 以gbk的形式创建数据
create database mydatabase2 charset gbk; 
```

**显示数据库：**

数据库没有指定字符集，那么就会使用DMBS默认的字符集 

```mysql
-- 显示数据库
show databases;
-- 基本语法  showdatabases like '匹配模式';
-- _ :匹配当前位置单个字符
-- % ：匹配指定位置多个字符
-- 查看以my开头的数据库
show databases like 'my%';
```

查看数据库创建语句：

```mysql
show create database mydatabase;
```

选择数据库：

```mysql
use 数据库名称;
-- 当显示Database change时，表示已经进入
```

修改数据库：

```mysql
-- 修改字符集
alter database mydatabase charset gbk;
```

删除数据库：(对应文件夹也删除)

```mysql
 drop database mydatabase;
```



#### 普通创建表：

**创建数据表**

注意：表示必须放到对应的数据库下：

方法

1. 在数据表名字前面加上数据库名称  ，还有  '' .  ''  mydatabase2.class
2. 

格式：create table 表明（字段名 字段类型【字段属性】 ，字段名，字段类型【字段属性】。。。);

```mysql
create table mydatabase2.class(
name varchar(10) //超过10个字符
);
-- 在mydatabase2库下，创建class这个表
```

```mysql
use mydatabase2;
create table teacher(
name varchar(10)  
);
```

```mysql
use mydatabase2;
create table mydatabase.teach(
name varchar(10)  
);
```

第二个例子是在mydatabase库下创建的表，然后只有use的话，就是默认用的哪个库就在哪个库下创建

#### 表选项：与数据库选项类似

Engine：存储引擎，mysql提供的具体存储数据的方式

Charset：字符集，只对自己当前表有效，（级别比数据库高）

Collate：校对集

**使用表选项：**

```mysql
create table student(
name varchar(10)
)charset utf8;
```

**复制已有表结构：只复制结构，如果表中有数据，不复制数据**

```mysql
create table teach like mydatabase.teach;
```



**显示数据表**

```mysql
show tables
```

**显示表结构**

显示表中所含的字段信息（名字，名称，属性）

```mysql
Describe 表名
Desc  表名
show columns from 表名
```

* Field 字段名称

* Type 字段类型
* NULL  值是否允许为空
* Key  索引
* Default 默认值
* Extra  额外的属性

**显示表创建语句**

查看数据表创建时的语句；显示的是加工过的用户输入的内容

基本语法：

```mysql
-- 查看表创建语句
show create table 表名；
```

Mysql中的多种语句结束符，

；与 \g 所表示的效果是相同的,是字段在上面排横着，下面跟对应的数据

\G 字段在左侧竖着，数据在右侧横着



**设置表属性、修改表选项**

表属性指的是表选项： 

Engine：存储引擎，mysql提供的具体存储数据的方式

Charset：字符集，只对自己当前表有效，（级别比数据库高）

Collate：校对集

基本语法：

```mysql
alter table 表选项 [=] 值
alter table student charset gbk;
```

注意：如果数据库已经确定，并且存入数据，不要轻易修改表选项（字符集影响不大）

**修改表名：**

```mysql
 -- 注意，在修改的时候，一定要注明是表还是库
 rename table student to my_student;
```

**增加字段：**

```mysql
alter table 表名 add[column] 新字段名 列类型[列属性][位置first/after字段名]
-- 给学生表增加age字段,当没有first或者after的时候 默认是加到表的后面
alter table my_student add column age int; 
-- 给学生表增加a字段，加在表的前面
alter table my_student add column a int first;
```

**修改字段名：**    **注意一定要标注新字段名的字段类型**

```mysql
alter table 表名 change 旧字段名 新字段名 字段类型 [列属性][新位置]
-- 将列表student中的 a 字段修改成 aa 
alter table my_student change a aa int;
```

**修改字段类型的时候，change 换成 modify**

```mysql
alter table my_student modify aa varchar(20);
```

**删除字段**

```MYSQL
alter table my_student drop aa;
```

**删除表结构**

```mysql
drop table 表名;
当要删除多个时候，drop table 表名1,表名2;
```



#### 数据基础操作

**插入操作：，本质是以SQL的形式存储到指定的数据表中**

#### 在插入的时候，一定要先增加字段！！！，格式按照desc 表名 的顺序来

**基本语法：**

```mysql
-- 插入数据到数据表
insert into 表名 (字段列表) values (对应字段列表)
insert into my_student (name,age) values ('Jack',30);

-- 向表中所有字段插入数据，此时值列表必须对应表结构字段列表
insert into my_student value('Li',28);
-- 在这里，创建my_student 的时候先有了name这个字段，如果要存储age的话，就在加一个字段就好了
```

注意：values中，对应的值列表只需要与前面的字段列表相对应即可（不一定与表结构完全一致）

**查询操作**

```mysql
-- 查询表中全部数据
select * from my_student;
-- 查询表中部分字段
select 字段列表 from my_studnet;
-- 简单条件查询数据,没有的话，返回empty
基本语法：select 字段列表 或者 * from 表名 where age =  30;
select name from class where name = 3;

```

**删除操作**

```mysql
基本语法：delete from 表名 [where 条件];当没有where的话，自动删除该表所有数据
 delete from class  where name = 1;
```

**更新操作**

```mysql
-- 基本语法：update 表名 set 字段名 = 新值[where]; 如果没有where，所有的表中对应的字段都会被修改成所设定的值
 update class set age = 28 where name = 'ww' ; 
-- ww 记得加 ' ' 
```



#### 字符集

**字符编码概念：字符在二进制中对应的编码**

计算机针对各种符号，在计算机中的一种二进制存储代号

**设置客户端所有字符集**

如果直接通过cmd下的mysql.ext进行中文数据插入，会出错

出错原因：**cmd下的mysql.exe默认都只有一个字符集：GBK**

1. 真正的SQL指令是Mysql.exe来执行的
2. 在输入的时候并没有告知Mysql.exe对应的符号规则，就会使用自己默认的字符集

解决方案：

  mysql.exe客户端在进行数据操作之前，将自己所用的字符集告诉mysql



快捷方式：

```mysql
set name 字符集;
set name gbk;(通过查看cmd命令，简体中文对应的是gbk)
insert into class values ('黄啊',12);
-- 这样就能正常插入了
```

深层原理：

客户端，服务器端，连接层

Mysqld.exe 和 Mysqld.exe 之间的处理分为三层

1. 客户端到服务器
2. 服务器端到客户端
3. 客户端与服务器之间的链接

而set name 字符集的本质，是一次性把这三层关系的字符集变得一致

**查看系统各部分对应的字符集**

```mysql
show variables like 'character_set%';
```

修改服务器端变量的值：

这里的名称是上面的查看方法所显示的

```mysql
set character_set_client = gbk; // 客户端到服务器
set character_set_connection = gbk; // 更好的帮助客户端于服务器之间的字符集转换
set character_set_result = gbk; // 告诉客户端服务端所有的返回数据字符集类型
```

