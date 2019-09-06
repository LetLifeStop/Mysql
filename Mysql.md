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
   - -h 指定客户端所要登录的Mysql主机名，可省略，默认是local post
   - -u 登录的用户名
   - -p通过密码登录
   - -P 端口号
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

注意：表示必须放到对应的数据库下

方法

1. 在数据表名字前面加上数据库名称  ，还有  '' .  ''  mydatabase2.class
2. 格式：create table 表明（字段名 字段类型【字段属性】 ，字段名，字段类型【字段属性】。。。);

**多个类型之间通过，隔开，然后最后一个类型后面与）之间无符号**

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

- Field 字段名称
- Type 字段类型
- NULL  值是否允许为空
- Key  索引
- Default 默认值
- Extra  额外的属性

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

Charset：字符集，只对自己当前表有效（级别比数据库高）

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



#### 列类型（字段类型）

一个字节是8比特，int类型是4个字节，然后就是 2 ^ 32 这样来推断 

 **整数类型**

1. Tinyint （一个字节）
2. Smallint （两个字节）
3. Mediumint （三个字节）
4. Int （四个字节）
5. Bigint （八个字节）

```mysql
create table my_int(
int_1 tinyint,
int_2 smallint,
int_3 mediumint,
int_4 int,
int_5 bigint
)charset utf8;
```

**插入数据**

```mysql
insert into my_int (int_1,int_2,int_3,int_4,int_5) values (10,100,1000,10000,100000);
```

当插入的范围超过该类型定义的范围的时候，会以补码的形式存在



**unsigned类型，在相应的类型后面加上unsigned** 

```mysql
create table my_int2(
int_1 tinyint unsigned,
int_2 smallint,
int_3 mediumint,
int_4 int,
int_5 bigint
)charset utf8;
```



**显示长度**

指数据在显示的时候，到底可以显示多长位

Tinyint(3) 表示最长可以显示3位 ，但是显示长度只是代表了数据是否可以达到指定的长度，但是不会自动补零。如果还要自动补零，还需要给字段加一个 zerofill 属性

zerofill：从左侧开始填充0，（负数的时候无法使用），**一旦使用zerofill就相当于确定该字段为unsigned类型**



**小数类型**

分为浮点型，定点型

**浮点型**

精度类型，是一种有可能丢失精度的数据类型，尤其是在超出精度长度的时候

- float

称之为单精度类型，系统提供4个字节来存储数据，但是能表示的范围比整形大，大约是10^38，只能保证数据在7位数以内是精确的

基本语法：

```mysql
float    不指定小数位的浮点数
float(10,2)  整数部分为8位，小数部分为2位
```

创建一个数据表保存浮点数据：

注意：

 **For float(M,D), double(M,D) or decimal(M,D), M must be >= D** 

**创建：**

```mysql
create table my_float(
f1 float,
f2 float(10,2)
)charset utf8;
```

**存储：**

```mysql
insert into my_float (f1,f2) values (1.2,2.3);
```

1. **如果数据精度丢失，浮点型按照四舍五入的方法进行计算**
2. 用户不能插入数据直接超过指定的整数部分长度，但是如果是系统自动进位，这个是允许的。比如  999.9 进位到 1000
3. 浮点数可以采用科学计数法来存储数据



- double 

被称为双精度，采用8个字节来存储数据，表示的范围更大， 10 ^ 308 ,但是精度只有15位左右



**定点数：**

能够保证数据精确的小数（**整数部分一定精确，但是小数部分不一定精确**）

- Decimal

系统根据存储的数据来分配存储空间，每大概9个数就会分配四个字节来存储，同时小数部分和整数部分是分开的, **如果整数部分进位，超出长度也会报错**

格式：

Decimal(M,D);

M 表示总长度，最大值不能超过65. D代表小数部分长度，最长不能超过30

1. 创建表

```mysql
create table my_dec2(
f1 float(10.2),
f2 decimal(10,2)
)charset utf8;
```

插入正常数据

```mysql
insert into my_dec values(12345678.90,12345678.90);
```



**时间日期类型**

* Date

日期类型，对应格式 YYYY-mm-dd。范围 从1000-01-01 ~ 9999 -12 -12

* Time

时间类型，能够制定某个具体的时间段，表示格式 HH：ii：ss，但是系统提供了3个字节来存储，具体用处是描述时间段 ， 范围 从 -838:59:59 ~ 838:59:59 

* Datetime

日期时间类型，将前两种合起来 是 YYYY-mm-dd HH:ii:ss .一共是八个字节存储数据。表示的区间范围 1000-01-01 00：00:00 ~ 9999:12:12 23.59:59

* Timestamp

时间戳类型，mysql中的时间戳是从格林威治时间开始的  ，格式为 YYYY-mm-dd HH:ii:ss 

* Year

年类型，占用一个字节来保存，能表示1900 ~ 2155年



**创建对应的时间日期类型的数据表**

```mysql
create table my_data2(
d1 date,
d2 time,
d3 datetime,
-- 如果要时间戳自动更新的话，就需要加选项 
d4 timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP, 
d5 year
)charset utf8;
```



**ps：datatime和 timestamp 的区别**

* datatime 

1. 允许为空值，可以自定义值，系统不会修改其值
2. 不可以设定默认值，所以在不允许为空的情况下，必须手动指定database字段的值才能成功插入数据
3. 虽然不可以设定默认值，但是可以在指定datatime字段的值时，使用now变量来自动插入系统当前的时间

* timestamp

1. 允许为空值，但是不可以定义值，空值是没有任何意义
2. 默认值为 CURRENT_TIMESTAMP( )，就是当前的系统时间
3. 数据库会自动修改这个值，所以在插入记录的时候只需要在设计表上添加一个timestamp字段就可以，不需要指定timestamp字段的名称 和 timestamp字段的值，插入该字段的值会自动变成当前的系统时间
4. 当修改记录表中的值的时候，对应记录表的timestamp值会自动更新为当前系统的时间



**插入数据**

```mysql
insert into my_data (d1,d2,d3,d4,d5) values ('1900-01-01','12:12:12','1900-01-01 12:12:12','1999-01-01 12:12:12',69);
```

注意：year既可以采用两位数的数据插入，也可以采用4位数；当year按照两位数来进行插入的时候， <= 69, 默认年份是20XX；当 >=70 的时候，默认年份是 19XX；



**更新数据**

```mysql
update my_data set d1 = '1905-01-01' where d5 ='2069';
```



在进行时间类型的录入的时候，还可以进行一个简单的日期代替时间

```mysql
-- 在当前时间往后加5天
5 12:12:12
```



**字符类型**

**在utf8中，一个字符都会占用3个字节**

**char**

定长字符：指定长度后，系统会分配指定的空间用于存储数据结构

基本语法：char(L) ，L长度为 0 ~ 255

**varchar**

变长字符：指定长度后，系统会根据实际存储的数据来计算长度，分配合适的长度

（数据没有超出长度）

基本语法：Varchar（L）。L表示字符数，L的理论长度为 0 ~ 65535

**因为varchar要记录数据长度，所以每个varchar数据产生后，系统都会在数据后面增加 1-2个字节的开销；是用来保存数据所占用的空间长度，如果数据本身小于 127个字符，额外开销一个字节，如果大于127个字符，额外开销两个字节**



**char 和 varchar 的区别：**

1. char 定空间是通过指定，而varchar是通过数据来定空间
2. char 的数据查询效率比varchar高



ps：

* 如果确定数据一定是占指定长度，那么使用char类型，否则使用varchar类型

* 如果数据长度超过255个字符，不论是否固定长度，都要用text，不再使用char和varchar



**Text**

文本类型，mysql 提供了两大种文本类型，text 和 blob 。

text：存储普通的字符文本

blob：存储二进制文件



**text分类：**

* tinytext ，一个字节存储  实际存储为 2^8 + 1
* text  两个字节存储   实际存储为 2^16 + 2  
* mediumtext: 使用三个字节存储，实际存储为 2^ 24 + 3
* longtext ：使用四个字节存储，实际存储为 2^32 + 4



**注意：**

在选择对应的存储文本的时候，不用刻意去选择text类型，系统会自动根据存储的数据长度来选择合适的文本类型



**Enum** 

枚举类型，在数据插入之前，先设定几个项，这几个项就是可能最终出现的数据结果。

基本语法：

enum ( 数据值1，数据值2....)

ps：系统提供了1到2个字节来存储枚举数据结构，通过计算enum列举的具体值来分配空间

 <= 255 一个字节；>= 255 && <= 65535 ，系统采用两个字节保存



**创建表**

```mysql
create table my_enum(
lala enum('男','女','保密')
)charset utf8;
```

**插入数据**

```mysql
insert into my_enum values('男');
-- 这是错误的
insert into my_enum values('我');
```

enum有规范数据的功能，能够保证插入的数据必须是设定的范围

**枚举的存储原理：**

字段上存储的值并不是真正的字符串，而是一个下标，相当于对这个字符串map映射了一下，每一次输进来的数先判断是不是在范围内，然后再判断是哪个下标（从1开始）

```mysql
-- 查看方法
select 字段名 + 0 from 表名;
select lala +  0 from my_enum;
```

同理，在插入的时候也可以通过下标来进行插入

```mysql
insert into my_enum values(1);
```

**枚举的意义**

* 规范数据本身，限定只能插入规定的数据项
* 节省内存空间



**Set**

集合，将多个数据选项可以同时保存的数据类型，本质是按照指定的项对应的二进制位来进行控制，1表示该选项被选中

**基本语法：**

set ('值1','值2','值3’,,,,);

系统通过自定计算来选择具体的存储单元

1字节-> 8个选项

2字节-> 16个选项

3字节->24个选项

8字节->set可以表示64个选项 

**set和enum运行原理相同，最终存储到数据字段的依旧是数字而不是真正的字符串**

1. 创建表

```mysql
create table my_set(
hk set('黄啊','就是','这个样','啊啊啊啊','噗哈哈哈哈哈')
)charset utf8;
```

2. 插入数据，可以插入多个数据，当不是set的时候，无法插入

```mysql
insert into my_set values('黄啊,啊啊啊啊');
```

3. 插入的数据会自动按照set中的值自动排序

4. 数据存储方式，通过二进制的形式来实现。当我们插入多个数的时候，对应原来的set里面的值，如果存在则二进制为1 ，否则为0.然后将获得的二进制倒过来，再转换成十进制，这个就是此次插入的value 的值。也就是说我们可以直接插入这个10进制的数来直接插入这两个元素

**set集合的意义**

1. 规范数据  2. 节省存储空间



#### 列属性

列属性也称为字段属性，mysql中一共有6个属性：null，默认值，列描述，主键，唯一键 和 自动增长



* NULL

NULL属性：代表字段为空，如果对应的NULL为YES表示该字段可以为空

注意：

1. 在设计表的时候，尽量不要让数据为空
2. Mysql 的 记录长度为 65535个字节，如果一个表中允许为NULL，那么系统就会设计保留一个字节来存储NULL，最终有效存储长度为 65534个字节



* 默认值

default：默认值，当字段被设计的时候，如果允许默认的条件下，用户不进行数据的插入，那么就可以使用提前准备好的数据来填充，通常填充的是NULL

```mysql
create table my_default(
-- NULL 不能为空
name varchar(10) NOT NULL,
-- 如果当前字段在进行数据插入的时候没有提供数据，默认值为 18
age int default 18
)charset utf8;
```

default 的另一个用法：

```mysql
-- 使用默认值进行插入
insert into my_default (name,age) values ('dawd',default);
```



* 列描述

comment，专门用于给开发人员进行维护的一个注释说明

基本语法： comment '字段描述'

```mysql
create table my_comment(
name varchar(10) not null comment '当前为用户名，不能为空',
pass varchar(50)  comment '密码可以为空'
)charset utf8;
```



* 主键

primary key ，主要的键，**在一张表上，有且只有一个字段**，里面的值具有唯一性

PRI 主键约束；UNI 唯一约束； MUL 可以重复；**主键的NULL 为 NO**



创建方法：

方法1，直接在需要当做主键的字段之后，增加primary key 属性来确定主键

```mysql
create table my_key1(
username varchar(10),
primary key(username)
)charset utf8;
```

方法2，在所有字段之后增加primary key 选项：primary key(字段信息)

```mysql
create table my_key2(
username varchar(10) primary key
)charset utf8;
```

方法3.

**创建表之后**增加主键：

基本语法： alter table 表名 add primary key(字段)；

```mysql
create table my_key3(
username varchar(10)
-- primary key(username)
)charset utf8;

alter table my_key3 add primary key (username);
```



查看主键：

```mysql
-- 查看表的结构
desc my_key3;
-- 查看表的创建语句
show create table my_key3;
```



删除主键：

```mysql
alter table 表名 drop primary key;
alter table my_key3 drop primary key;
```



复合主键

表的主键含有一个以上的字段组成desc 

```mysql
create table my_key4(
username varchar(10),
teachname varchar(10),
primary key(username,teachname)
)charset utf8;
```

主键一旦增加，那么对应的字段有数据要求

1. 当前字段对应的数据不能为空
2. 当前字段对应的数据不能有任何重复

主键分类：

主键分类采用的是主键所对应的字段的业务意义分类：

业务主键：主键所在的字段，具有业务意义（学生ID，课程ID）

逻辑主键：自然增长的整形（应用广泛）



* 唯一键

unique key , 用来保证对应的字段中的数据唯一   **NULL值可以为YES ，注意可主键区分**

主键也可以用来保证字段数据唯一性，但是一张表只有一个主键

1. 唯一键在一张表中可以有多个
2. 唯一键允许字段数据为NULL，NULL可以有多个（NULL不参与比较）
3. 在不为空的情况下，不允许重复



创建唯一键

方法和创建主键的方法非常相似

方法1，直接在表字段之后增加唯一键标识符：unique[key]

```mysql
create table my_unique1(
username varchar(10) unique
)charset utf8;
```

方法2，在所有的字段之后使用unique key(字段列表)

```mysql
create table my_unique2(
username varchar(10),
unique key(username)
)charset utf8;
```

方法3，表创建完之后增加唯一键

```mysql
create table my_unique3(
username varchar(10)
-- unique key(username)
)charset utf8;

alter table  my_unique3 add unique key(username);
```



查看唯一键：

唯一键是属性，可以通过查看表结构来实现



删除唯一键：

一个表中允许存在多个唯一键

```mysql
alter table my_unique3 drop index username;
```

只是唯一键消失了，但是这个数据类型还是存在的



修改唯一键：

先删除，后增加



复合唯一键

唯一键与主键一样可以使用多个字段来共同保证唯一性

**一般主键都是单一字段（逻辑主键），而其他需要唯一性的内容都是由唯一键来处理的？？**



* 自动增长

auto_increment，通常用于逻辑主键， 当给定某个字段该属性之后，该列的数据在没有提供确定数据的时候，系统会根据之前已经存在的数据进行自动增加，填充数据



原理：

1. 在系统中会维护一组数据，用来保存当前使用了自动增长属性的字段，记住当前对应的数据值，再给定一个指定的步长
2. 当用户进行数据插入的时候，如果没有给定值，系统在原始值上再加上步长变成新的数据
3. 自动增长的触发方式：给定属性的字段没有提供值
4. 自动增长只适用于数值



使用自动增长：

基本语法：

方法1，在字段之后增加属性 auto_increment 

**Incorrect table definition; there can be only one auto column and it must be defined as a key**

只能有一列是自动增长类型，并且需要先定义为key类型

```mysql
create table my_auto(
id int primary key auto_increment
)charset utf8;
```

方法2，创建之后添加属性

```mysql
alter table 表名 change 旧字段名 新字段名 字段类型 [列属性][新位置]
alter table my_auto change id id int not null auto_increment;
-- 这里可以多说一点，not null 是因为 auto_increment 的前提是为key，然后key的前提是null不为空
```



插入数据：

```mysql
-- 如果要使用自动增长，不能给定具体值
insert my_auto values(null ,1);
```



查看自增长：

```mysql
-- 自增长在触发之后，会自动的在表选项中添加一个选项（一张表最多只能拥有一个自增长）
show create table 表名;
```



修改自增长：

```mysql
-- 基本语法
alter table 表名 auto_increment = 值;
```



删除自增长：

在该字段属性之后不再保留auto_increment，当用户修改自增长所在字段时，如果没有看到auto_increment 属性，系统会自动清除该自增长

```mysql

alter table my_auto modify id int;
```



ps:

1. 一张表只有一个自增长，自增长会上升到表选项中

```mysql
-- 查看自增长初始变量
show variables like 'auto_increment%';
```

2. 如果数据表中没有触发自增长，那么自增长不会表现
3. 自增长修改的时候，值可以较大，但是必须必当前已有的字段的自增长值大



**表关系**

表与表之间有什么关系，每种关系应该如何设计表结构



* 一对一

一张表中的一条记录与另外一张表中最多有一条明确的关系；通常，这个设计方案保证两张表中使用相同的主键

比如，一个学生表如下：

| 学生ID（PRI） | 姓名 | 年龄 | 性别 | 籍贯 | 婚否 | 住址 |
| ------------- | ---- | ---- | ---- | ---- | ---- | ---- |
|               |      |      |      |      |      |      |

然后我们经常查询的有后三个，而前三个很少查询到，我们可以将后三个和前三个分成两张表

常用表：

| 学生ID（PRI） | 籍贯 | 婚否 | 住址 |
| ------------- | ---- | ---- | ---- |
|               |      |      |      |

不常用表：

| 学生ID（PRI） | 姓名 | 年龄 | 性别 |
| ------------- | ---- | ---- | ---- |
|               |      |      |      |



* 一对多

通常也叫多对一，在一对多的关系设计方案中，大多数情况是 在 ‘多’ 的关系中去维护一个字段，这个字段时 ‘一’关系的主键

如下：

在‘一’中，也就是孩子表中，主键是母亲ID；而在‘多’中，也就是母亲表中，维护的字段也是母亲Id。

母亲表

| 母亲ID | 姓名 | 年龄 | 身高 |
| ------ | ---- | ---- | ---- |
| M1     |      |      |      |
| M2     |      |      |      |

 

孩子表

| 孩子ID | 姓名 | 年龄 | 身高 | 母亲ID |
| ------ | ---- | ---- | ---- | ------ |
| K1     |      |      |      | M1     |
| K2     |      |      |      | M1     |

 

* 多对多

多对多：一张表中的一条记录在另外一张表中可以匹配到多条记录，反过来也一样。

多对多的关系如果按照多对一的关系维护，就会出现一个字段中有多个其他表的主键，在访问的时候就会带来不方便。

我们可以通过第三张表来解决，增加一个中间表，让中间表与其对应的其他表形成两个多对一的关系；多对一的解决方案是在 ‘多’ 表中增加 ‘一’ 表对应的主键字段

示例如图：

![1567561657435](C:\Users\acm506\AppData\Roaming\Typora\typora-user-images\1567561657435.png)







### 高级数据操作



#### 新增数据



* 多数据插入

只要写入一次insert指令，就可以直接插入多条记录

基本语法：insert into 表名 (字段列表) values （值列表) ,(值列表)...;

```mysql
insert into class (name,age) values ('huang',12),('qing',13),('xiang',14);  
```



* 主键冲突

因为主键要求主键的值具有唯一性，如果插入数据的时候，有相同的主键该如何处理？

1. 主键冲突更新

类似插入数据语法，插入的过程中主键重入，那么采用更新方法

```mysql
-- 语法
insert into 表名 (字段列表) values (值列表) on duplicate key update 字段 = 新值;

-- 将name为ww的对象 ，age改成28
 insert into class (name ,age) values ('ww',28) on duplicate key update age = 29;
```

2. 主键冲突替换

当主键冲突的时候，将原来的数据覆盖掉

```mysql
-- 语法
replace into 表名 (字段列表) values (值列表)；
-- 覆盖
replace into class values ('ww',123);
```



* 蠕虫复制

一分为二，成倍的增加。从已有的数据中获取数据，并且将获取的数据插入到数据表中

```mysql
-- 基本语法
insert into 表名 (字段列表) select * 或者 字段列表 from 表;

insert into class select * from class;
```



注意：

1.  蠕虫复制虽然是重复数据，但是可以用来测试表的压力，还可以通过大量数据来测试表的效率

2. 注意主键冲突



#### 更新数据

1. 在更新数据的时候，特别要注意，一般是跟随条件进行更新的

```mysql
-- 语法
update 表名 set 字段名 = 新值 where 判断条件 limit 数量;
--  where 判断条件 也是可以去掉的，这个时候最好限制数目
update 表名 set 字段名 = 新值 limit 数量;
update class set name = 'w' where name = 'ww' limit 2;
```



#### 删除数据

1. 删除数据的时候尽量不要全部删除，应该使用where进行判断；
2. 删除数据的时候可以使用limit来限制要删除的具体数目;

```mysql
delete from 表名 where 条件(当有多个条件的时候，可以使用 &&);
```

注意：

1. delete删除数据的时候无法重置 auto_increment
2. 重置表选项中的自增长的方法

```mysql
truncate 表名;
```



#### 查询数据

```mysql
-- 完整语法
select (select选项) 字段列表 from 数据源 where 条件 group by 分组 having 条件 order by 排序 limit 限制;
```

* select 选项

系统如何对待查询得到的结果

 all : 默认，表示保存所有的记录

distinct：去除重复的记录（所有的字段都相同），只保留一条

* 字段列表

有时需要从多张表获取数据，在获取数据的时候，可能存在不同表中有同名的字段，需要将同名的字段命名成不同命：

```mysql
-- 基本语法： 字段名 as 别名
-- 从class表中，找出没有重复的姓名数据，然后这个表中的name在当前的查询数据中对应的姓名是 name1,name2
select distinct name as name1,name as name2 from class;
```

* from  数据源

from是为了前面的查询提供数据：数据源只要是一个符合二维表结构的数据即可

```mysql
-- 单表数据
from 表名;
-- 多表数据
from 表一,表二;
select * from class1,class2;
```

结果：两张表的记录数相乘，字段数拼接

本质：从第一张表取出一条记录，去拼凑第二张表的所有记录，保留所有的结果。得到的结果在数学上成为：笛卡尔积 。会给数据库造成压力，应尽量避免出现笛卡尔积。



* 动态数组

from后面跟的结果不是一个实体表，而是一个从表中查询出来得到的二维结果表。

```mysql
-- 基本语法
from (select 字段列表 from 表名) as 新生成表的别名;
select * from (select int_1,int_3 from my_int) as int_my;
```



* where子句

用来从数据表中获取数据，然后进行条件筛选



* group by 子句

表示分组，根据指定的字段，将数据进行分组；分组的目标是为了方便统计

```mysql
select * from my_int group by int_1;
```

group 将数据按照指定的字段分组之后，只会保留每组的第一条记录



利用一些统计函数：

1. count() ：统计每组中的数量，如果统计目标是字段，那么不统计为NULL子段，如果括号里面填 * 代表统计记录
2. avg() : 平均值
3. sum(): 总和
4. max(): 最大值
5. min(); 最小值
6. group_concat() ：将分组中指定的字段进行合并（字符串拼接）

* 多分组

将数据按照某个字段进行分组之后，对已经分组的数据进行再次分组

基本语法：

```mysql
-- 先按照字段一进行分类，再按照字段二进行分类
group by 字段一,字段二;
```

* 分组排序

基本语法：

```mysql
-- 将字段一按照升序排序，字段二进行降序排序
group by 字段一 asc , 字段二 desc;
```

* 回溯统计

当分组进行多次的时候，在网上统计的过程中，需要进行层层上报，将这个过程称为回溯统计。每一次分组向上统计的过程都会产生一次新的统计数据，并且当前数据对应的分组字段为NULL.

基本语法：

```mysql
group by 字段 [asc|desc] with rollup;
```

* 多分组回溯统计

```mysql
select int_1,count(*) from my_int froup by int_1,int_2 with rollup;
```



* Having 子句

Having子句的本质和where是相同的，是用来进行数据条件筛选

1. Having是在group by 子句之后，可以根据统计结果筛选（where不能根据统计结果筛选）
2. where不能使用聚合函数，聚合函数是在group by 分组的时候，再去运行，但是这个时候where已经执行完毕

```mysql
select int_1 , count(*) as number from my_int group by int_2 having number >= 2 ;
-- 从my_int表中，查询字段int_1 ,然后判断该类的数目 按照int_2 进行排序，然后再对筛选完的结果取 >= 2 的个数 
```

**Having 是在group by 之后，group by 都是在where 之后，where 的时候是从磁盘拿到内存，where之后的所有操作都是内存操作**



* order by 子句

根据校对规则对数据进行排序

```mysql
-- 基本语法
order by 字段 [asc|desc];
-- 将获取的内容按照int_2 升序排列
select * from my_int order by int_2 asc;
```

也可以实现多字段排序

```mysql
order by 字段一,规则,字段二，规则;
select * from my_int order by int_2 asc,int_3 desc;
```



* limit子句

用来限制记录数量获取



* 分页

利用limit 来限制获取指定区间的数据

基本语法：

```mysql
-- offset 为偏移量，从哪里开始，如果没有的话默认为0
limit offset,length;
```



* between

```mysql
 -- 查找区间
 select * from my_int where int_1 between -1 and 3;
```



#### 查询中的运算符

* 算术运算符

​     \+ , - , * , / , %

基本算术运算： 通常不在条件中使用，而是用于结果运算（select 字段）

```mysql
select int_1 + int_2 ,int_1 - int_2 ,int_3 * int_4 ,int_3 / int_4 ,int_3 % int_4 from my_int;
```

* 比较运算符

\>= , < , <= , = ,<

通常用来在条件中进行限定结果

* 逻辑运算符

and , or , not 

* in运算符

当结果是一个集合的时候，用来替代 = 

```mysql
-- 基本语法
in(结果1,结果2,结果3...)只要当前结果在集合中出现过，那么就成立
select * from my_int where int_2 in (2,2);
```

* like运算符

是用来进行模糊匹配的（字符串）

```mysql
-- 基本语法  like‘匹配子串’  % 表示 单个字符 ， _表示多个字符
select * from my_int where int_1 like '_';
select * from my_int where int_1 like '%';
```



#### 联合查询

**基本概念**

联合查询是可合并多个相似的选择查询的结果集。等同于将一个表追加到另一个表，从而实现将两个表的查询组合在一起，使用为谓词 为 UNION 或 UNION ALL 

联合查询：将多个查询的结果合并到一起（纵向合并）：字段数不变，多个查询的记录数合并

**应用场景**

1. 将同一张表中不同的结果（需要对多条查询语句来实现），合并到一起展示数据，男生身高升序女生身高降序
2. 最常见：在数据量大的情况下，会对表进行分类操作，主要对每张表进行部分数据统计，使用联合查询来将数据存放到一起

**基本语法**

select 语句

union [union 选项]

select 语句

union 选项：与select 选项基本一样

distinct：去重，去掉完全重复的数据（union 默认的是去重）

```mysql
select * from my_int
union 
select * from my_int;
```

在union后面加上 all ， 是保存所有结果



union理论只保证字段数一样，不需要每次拿到的数据对应的字段类型一样。永远只保留第一个select 语句对应的字段名字



* order by 的使用

在联合查询中，如果要使用order by ，那么对应的select 语句必须使用括号括起来

```mysql
(select * from my_int)
union 
(select * from my_int order by int_1 desc);
```



#### 连接查询

将多张表连到一起进行查询（会导致记录数 行 和 字段数列发生改变）

* 连接查询的意义

 在关系型数据库设计的过程中，表与表之间是存在很多联系，在关系型数据库的设计过程中，遵循着：一对多，一对一，多对多。在实际的操作过程中，通过这层关系来保证数据的完整性

* 分类

交叉连接、内连接、外连接（左外连接（左连接），右外连接（右连接））、自然连接

* 交叉连接

将两张表的数据与另外一张表彼此交叉

**原理**

1. 从第一张表一次取出每一条记录
2. 取出每一条记录之后，与另外一张表的全部记录挨个匹配
3. 没有任何匹配条件，所有的结果都会进行保存
4. 记录数 = 第一张表记录数 * 第二张表记录数 ；字段数 = 第一张表字段数 + 第二张表字段数(笛卡尔积)

```mysql
-- 基本语法
表1 cross join 表2;
select * from my_int1 cross join my_int2;
```

**应用**

交叉连接产生的结果是笛卡尔积，没有实际应用

本质： from 表1 ,表2;



* 内连接

inner join ，从一张表中取出所有的记录去另外一张表中匹配，利用匹配条件进行匹配，如果成功则保留，失败则放弃

**原理**

1. 从第一张表中取出一条记录，然后去另外一张表中进行匹配
2. 利用匹配条件继续匹配
3. 匹配到：保留，继续往下匹配
4. 匹配失败：向下继续， 如果全表匹配失败，结束

```mysql
-- 基本语法
-- 表1 inner join 表2 on 匹配条件;
-- 1. 如果内连接没有条件，那么其实就是交叉连接
select * from my_int inner join my_int2;
-- 2. 使用匹配条件进行匹配 ,注意字段前面要注明是哪个表的
 select * from my_int inner join my_int2 on my_int.int_2 = my_int2.int_2;

-- 如果条件中使用到对应的表名，而表名通常比较长，所以可以通过表别名来简化
 select * from my_int as int1 inner join my_int2 int2 on int1.int_2 = int2.int_2;
```

