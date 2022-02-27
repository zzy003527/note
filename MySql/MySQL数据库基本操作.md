## MySQL数据库基本操作

#### DDL

- DDL解释

  - DDL（Data Definition Language），数据定义语言，该语言部分包括以下内容：
    - 对数据库的常用操作
    - 对表结构的常用操作
    - 修改表结构

- 对数据库的常用操作

  - | 功能                       | SQL                                                 |
    | -------------------------- | --------------------------------------------------- |
    | 查看所有的数据库           | show databases;                                     |
    | 创建数据库                 | create database[if not exists] mydb1 [charset=utf8] |
    | 切换（选择要操作的）数据库 | use mydb1;                                          |
    | 删除数据库                 | drop database [if exists] mydb1;                    |
    | 修改数据库编码             | alter database mydb1 character set utf8;            |

    

- 对表结构的常用操作-创建表

  - 创建表格式

    - ```sql
      create table [if not exists]表名（
        字段名1  类型[(宽度)]  [约束条件]  [comment ’字段说明‘]，
        字段名2  类型[(宽度)]  [约束条件]  [comment ’字段说明‘]，
        字段名3  类型[(宽度)]  [约束条件]  [comment ’字段说明‘]，
      )[表的一些设置]；
      ```

    - **创建表是构建一张空表，指定这个表的名字，这个表有几列，每一列叫什么名字，以及每一列存储的数据类型**

    - ```sql
      use mydb1;
      create table if not exists student(
          sid int,
          name varchar(20),
          gender varchar(20),
          age int,
          birth date,
          address varchar(20)
      );
      ```

  - 数据类型

    - 数据类型是指在创建表的时候为表中字段指定数据类型，只有数据复合类型要求才能存储起来，使用数据类型的原则是：够用就行，尽量使用取值范围小的，而不用大的，这样可以更多的节省存储空间

    - 数值类型

      | 类型         | 大小    | 范围（有符号）                                       | 范围（无符号）                                        | 用途           |
      | ------------ | ------- | ---------------------------------------------------- | ----------------------------------------------------- | -------------- |
      | TINYINT      | 1 byte  | (-128,127)                                           | (0,255)                                               | 小整数值       |
      | SMALLINT     | 2 bytes | (-32768,32767)                                       | (0,65535)                                             | 大整数值       |
      | MEDIUMINT    | 3 bytes | (-8388608,8388607)                                   | (0,16777215)                                          | 大整数值       |
      | INT或INTEGER | 4 bytes | (-2147483648,2147483647）                            | (0,4294967295)                                        | 大整数值       |
      | BIGINT       | 8 bytes | (-9223372036854775808,9223372036854775807)           | (0,18446744073709551615)                              | 极大整数值     |
      | FLOAT        | 4 bytes | (-3.402823466 E+38,3.402823466351 E+38)              | 0,(1.175494351 E-38,3.402823466351 E+38)              | 单精度浮点数值 |
      | DOUBLE       | 8 bytes | (-1.7976931348623157 E+308,1.7976931348623157 E+308) | 0,(2.2550738585072014 E-308,1.7976931348623157 E+308) | 双精度浮点数值 |
      | DECIMAL      |         | 依赖于M和D的值                                       | 依赖于M和D的值                                        | 小数值         |

      

    - 日期和时间类型

    - | 类型          | 大小（bytes） | 范围                                                         | 格式                 | 用途                     |
      | ------------- | ------------- | ------------------------------------------------------------ | -------------------- | ------------------------ |
      | **DATE**      | 3             | 1000-01-01/9999-12-31                                        | YYYY-MM-DD           | 日期值                   |
      | TIME          | 3             | '-838:59:59'/'838:59:59'                                     | HH:MM:SS             | 时间值或持续时间         |
      | YEAR          | 1             | 1901/2155                                                    | YYYY                 | 年份值                   |
      | **DATETIME**  | 8             | 1000-01-01 00:00:00/9999-12-31 23:59:59                      | YYYY-MM-DD  HH:MM:SS | 混合日期和时间值         |
      | **TIMESTAMP** | 4             | 1970-01-01 00:00:00/2038  结束时间是第2147483647秒，北京时间2038-1-19 11：14：07，格林尼治时间2038年1月19日凌晨03：14：07 | YYYYMMDD  HHMMSS     | 混合日期和时间值，时间戳 |

      

    - 字符串类型

      | 类型       | 大小               | 用途                          |
      | ---------- | ------------------ | ----------------------------- |
      | CHAR       | 0-255 bytes        | 定长字符串                    |
      | VARCHAR    | 0-65535 bytes      | 变长字符串                    |
      | TINYBLOB   | 0-255 bytes        | 不超过255个字符的二进制字符串 |
      | TINYTEXT   | 0-255 bytes        | 短文本字符串                  |
      | BLOB       | 0-65535 bytes      | 二进制形式的长文本数据        |
      | TEXT       | 0-65535 bytes      | 长文本数据                    |
      | MEDIUMBLOB | 0-16777215 bytes   | 二进制形式的中等长度文本数据  |
      | MEDIUMTEXT | 0-16777215 bytes   | 中等长度文本数据              |
      | LONGBLOB   | 0-4294967295 bytes | 二进制形式的极大文本数据      |
      | LONGTEXT   | 0-4294967295 bytes | 极大文本数据                  |

      

  - 对表结构的常用操作——其他操作

    - | 功能                       | SQL                     |
      | -------------------------- | ----------------------- |
      | 查看当前数据库的所有表名称 | show tables;            |
      | 查看指定某个表的创建语句   | show create table 表名; |
      | 查看表结构                 | desc 表名               |
      | 删除表                     | drop table 表名         |

  - 对表结构的常用操作-修改表结构格式

    - 修改表添加列

      - 语法格式：

        ```sql
        alter table 表名 add 列名 类型（长度） [约束]；
        
        例如：#为student表添加一个新的字段为：系别dept类型为varchar（20）
        ALTER TABLE student ADD 'dept' VERCHAR(20);
        ```

    - 修改列名和类型

      - 语法格式：

        ```sql
        alter table 表名 change 旧列名 新列名 类型（长度） 约束；
        
        例如：# 为student表的的dept字段更换为department varchar（30）
        ALTER TABLE student change 'dept' department VARCHAR(30);
        ```

    - 修改表删除列

      - 语法格式：

        ```sql
        alter table 表名 drop 列名；
        
        例如：删除student表中department这列
        ALTER TABLE student DROP department;
        ```

    - 修改表名

      - 语法格式：

        ```sql
        rename table 表名 to 新表名；
        
        例如：# 将表student改名成stu
        rename table 'student' to stu;
        ```





#### DML

- DML介绍

  - DML是指数据操作语言，英文全称是Data Manipulation Language, 用来对数据库中表的数据记录进行更新。
  - 关键字：
    - 插入insert
    - 删除delete
    - 更新update

- 数据插入

  - 语法格式：

    ```sql
    格式1：insert into 表 (列名1，列名2，列名3...)  values （值1，值2，值3...）；       //向表中插入某些数据
    格式2：insert into 表 values（值1，值2，值3...）；         //向表中插入所有列
    
    例如：
    insert into student(sid,name,gender,age,birth,address,score) values (1001,'张三','男',18,'1996-12-23','北京',83.5);
    insert into student values (1001,'张三','男',18,'1996-12-23','北京',83.5);
    ```

- 数据修改

  - 语法格式：

    ```sql
    update 表名 set 字段名=值，字段名=值...;
    update 表名 set 字段名=值，字段名=值... where 条件;
    
    例如：-- 将所有学生的地址修改为重庆
    update student set address = '重庆';
    -- 将id为1004的学生的地址修改为北京
    update student set address = '北京' where id = 1004;
    -- 将id为1005的学生的地址修改为北京，成绩修改为100
    update student set address = '广州'，score = 100 where id = 1005;
    ```

- 数据删除

  - 语法格式：

    ```sql
    delete from 表名 [where 条件];
    truncate table 表名 或者 truncate 表名
    
    例如：-- 删除sid为1004的学生数据
    delete from student where sid = 1004;
    -- 删除表所有数据
    delete from student;
    -- 清空表数据
    truncate table student;
    truncate student;
    ```

  - **注意：delete和truncate原理不同，delete只删除内容，而truncate类似于drop table，可以理解为是将整个表删除，然后再创建该表**





#### MySQL约束

- 概念：约束实际上就是表中数据的限制条件

- 作用：表在设计的时候加入约束的目的就是为了保证表中的记录完整性和有效性，比如用户表有些列的值（手机号）不能为空，有些列的值（身份证号）不能重复

- 分类：

  - 主键约束（primary key）PK
  - 自增长约束（auto_increment）
  - 非空约束（not null）
  - 唯一性约束（unique）
  - 默认约束（default）
  - 零填充约束（zerofill）
  - 外键约束（foreign key）FK

- 主键约束

  - 概念

    - MySQL主键约束是一个或者多个列的组合，其值能唯一地标识表中的每一行，方便再RDBMS中尽快的找到某一行
    - 主键约束相当于 唯一约束 + 非空约束 的组合，**主键约束列不允许重复，也不允许出现空值**
    - 每个表最多只允许一个主键
    - 主键约束的关键词是：primary key
    - 当创建主键的约束时，系统默认会在所在的列和列的组合上建立对应的唯一索引

  - 操作

    - 添加单列主键
    - 添加多列联合主键
    - 删除主键

  - 添加单列主键

    - 创建单列主键有两种方式，一种是在定义字段的同时指定主键，一种是定义完字段之后指定主键

    - 方法1：

      ```sql
      -- 在 create table 语句中，通过PRIMARY KEY 的关键字来指定主键
      -- 在定义字段的同时指定主键，语法格式如下：
      create table 表名（
         ...
         <字段名><数据类型> primary key
         ...
      ）
      
      例子：
      create table emp1(
          eid int primary key,
          name VARCHAR(20),
          deptId int,
          salary double
      );
      ```

    - 方法2：

      ```sql
      -- 在定义字段之后再指定主键，语法格式如下：
      create table 表名（
         ...
         [constraint<约束名>]primary key[字段名]
      ）;
      
      例子：
      create table emp2(
          eid INT,
          name VARCHAR(20),
          deptId INT,
          salary double,
          constraint pk1 primary key(eid)        -- constraint pk1可以省略
      );
      ```

  - 添加多列主键（联合主键）

    - 所谓的联合主键，就是这个主键是由一张表中多个字段组成的

    - 注意：

      - 当主键是由多个字段组成时，不能直接在字段名后面声明主键约束
      - 一张表只能有一个主键，联合主键也是一个主键

    - 语法：

      ```sql
      create table 表名（
          ...
          primary key （字段1，字段2，...，字段n）
      ）；
      
      例子：
      create table emp3(
          name varchar(20),
          deptId int,
          salary double,
          primary key(name,deptId)   或   constraint pk2 primary key(name,deptId)
      );
      ```

    - 联合主键的各列，每一列都不能为空

    - 只要联合主键的各字段不完全相同即可

  - 通过修改表结构添加主键

    - 主键约束不仅可以在创建表的同时创建，也可以在修改表时添加

    - 语法：

      ```sql
      create table 表名（
          ...
      ）；
      alter table <表名> add primary key （字段列表）；
      
      例子：
      create table emp4(
          eid int,
          name varchar(20),
          deptId int,
          salary double
      );
      alter table emp4 add primary key (eid);       ——添加单列主键
      alter table emp4 add primary key (eid，deptId);           ——添加联合主键  
      ```

  - 删除主键约束

    - 一个表中不需要主键约束时，就需要从表中将其删除。删除主键约束的方法要比创建主键约束容易的多。

    - 格式：

      ```sql
      alter table <数据表名> drop primary key；
      
      例子：
      -- 删除单列主键
      alter table emp1 drop primary key;
      -- 删除联合主键
      alter table emp5 drop primary key;
      ```

      

- 自增长约束

  - 概念：在MySQL中，当主键定义为自增长之后，这个主键的值就不再需要用户输入数据了，而由数据库系统根据定义自动赋值。每增加一条记录，主键会自动以相同的步长进行增长。

  - 通过给字段添加**auto_incremen**t属性来实现主键自增长

  - 语法：

    ```sql
    字段名 数据类型 auto_increment
    
    例子：
    create table t_userl=1(
        id int primary key auto_increment,
        name varchar(20)
    );
    ```

  - 特点：

    - 默认情况下，auto_ _increment的初始值是1,每新增一条记录，字段值自动如1。
    - 一个表中**只能有一个字段**使用auto_ increment约束， 且该字段必须**有唯一 索引**，以避免序号重复(即为主键或主键的一部分)。
    - auto_ _increment约束的字段必须**具备NOT NULL属性**。
    - auto_ increment约束的字段只能是整数类型(TINYINT、 SMALLINT、 INT、 BIGINT） 等。
    - auto_ increment约束字段的最大值受该字段的数据类型约束,如果达到上限，auto_ increment就会失效

  - 指定自增字段初始值

    - 如果第一条记录设置了该字段的初始值，那么新增加的记录就从这个初始值开始自增。例如，如果表中插入的第一条记录的id值设置为5，那么再插入记录时，id值就会从5开始往上增加

    - 方式1：创建表时指定

      ```sql
      create table t_user2(
          id int primary key auto_increment,
          name varchar(20)
      )auto_increment=100;
      ```

    - 方式2：创建表之后指定

      ```sql
      create table t_user3(
          id int primary key auto_increment,
          name varchar(20)
      );
      alter table t_user3 auto_increment=100;
      ```

  - delete和truncate再删除后自增列的变化

    - delete数据之后自动增长从断点开始（从上一个值增长）
    - truncate数据之后自动增长从默认起始值开始

- 非空约束

  - MySQL非空约束（not null）指字段的值不能为空。对于使用了非空约束的字段，如果用户在添加数据时没有指定值，数据库系统就会报错。

  - 语法：

    ```sql
    方式1：<字段名><数据类型> not null；
    方式2：alter table 表名 modify 字段 类型 not null；
    
    例子（方式1）：
    create table t_user6 (
        id int,
        name varchar(20) not null,
        address varchar(20) not null
    );
    insert into t_user6(id,name,address) values(1001,NULL,NULL);   ——不可以
    insert into t_user6(id,name,address) values(1001,'NULL','NULL');    ——可以（字符串：NULL）
    insert into t_user6(id,name,address) values(1001,'','');             ——可以（空串） 
    
    
    例子（方式2）：
    create table t_user7 (
        id int,
        name varchar(20),
        address varchar(20)
    );
    alter table t_user7 modify name varchar(20) not null;
    ```

  - 删除非空约束

    - alter table 表名 modify 字段 类型

    - ```sql
      alter table t_user7 modify name varchar(20);
      ```

- 唯一约束

  - 唯一约束（Unique Key）是指所有记录中字段的值不能重复出现，例如，为id字段加上唯一性约束后，每条记录的id值都是唯一的，不能出现重复的情况。

  - 语法：

    - 方式1：创建表时指定：<字段名><数据类型>unique

      ```sql
      create table t_user8(
          id int,
          name varchar(20),
          phone_number varchar(20)unique -- 指定唯一约束
      );
      ```

    - 方式2：创建表之后指定：alter table 表名 add constraint 约束名 unique（列）

      ```sql
      create table t_user9(
          id int,
          name varchar(20),
          phone_number varchar(20)
      );
      alter table t_user9 add constraint unique_pn unique(phone_number);
      ```

      

  - 在MySQL中，NULL与任何值都不相同，甚至NULL不等于NULL

  - 删除唯一约束

    - 格式：alter table <表名> drop index <唯一约束名>

    - ```sql
      alter table t_user9 drop index unique_pn;
      ```

    - **若没有约束名，则列名为约束名**

      

- 默认约束（default）

  - MySQL默认值约束用来指定某列的默认值

  - 语法：

    - 方式1：<字段名><数据类型>default<默认值>;

      ```sql
      create table t_user10(
          id int,
          name varchar(20),
          address varchar(20)  default '北京'      ——指定默认约束
      );
      ```

    - 方式2：alter table 表名 modify 列名 类型 default 默认值;

      ```sql
      create table t_user11(
          id int,
          name varchar(20),
          address varchar(20)
      );
      alter table t_user11 modify address varchar(20) default '上海';
      ```

  - **对设置了默认约束的列赋值NULL，结果为NULL**

  - 删除默认约束

    - alter table <表名> modify <字段名><类型>  default null;

      ```sql
      alter table t_user11 modify address varchar(20) default null;
      ```

- 零填充约束（zerofill）

  - 概念

    - 插入数据时，当该字段的值的长度小于定义的长度时，会在该值的前面补上相应的0
    - zerofill默认为int（10）
    - 当使用zerofill时，默认会自动加unsigned（无符号）属性，使用unsigned属性后，数值范围是原值的2倍，例如，有符号为-128--+127，无符号为0--256

  - 操作

    - ```sql
      create table t_user12(
          id int zerofill    ——零填充约束
          name varchar(20)
      );
      ```

  - 删除

    ```sql
    alter table t_user12 modify id int;
    ```





#### DQL基本查询

- 简单查询

  - 概念：

    - 数据库管理系统一个重要功能就是数据查询，数据查询不应只是简单返回数据库中存储的数据，还应该根据需要对数据进行筛选以及确定数据以什么样的格式显示。
    - MySQL提供了功能强大、灵活的语句来实现这些操作
    - MySQL数据库使用select语句来查询数据

  - 语法格式：

    ```sql
    select
        [all|distinct]
        <目标列的表达式1>[别名]，
        <目标列的表达式2>[别名]...
        from <表名或视图名>[别名]，<表名或视图名>[别名]...
        [where<条件表达式>]
        [group by <列名>
        [having <条件表达式>]]
        [order by <列名> [adc|desc]]
        [limit <数字或者列表>];
    ```

  - 简化版语法：

    ```sql
    select * | 列名 from 表 where 条件
    
    select * from product;     -- 查询所有商品(全部查询)
    ```

  - 别名查询(使用的关键字是as)    (as可以省略的)

    - 表别名：**select** * **from** product **as** p;

      ​          或**select** * **from** product p;  

    - 列别名：**select** pname **as** '商品名' ,price  '商品价格‘ **from** product;

  - 去掉重复值

    - **select distinct** price **from** product;    (去重列数据)
    - **select distinct** * **from** product      （去重行数据 ）

  - 查询结果是表达式（运算查询）；将所有商品的加价10元进行展示

    - **select** pname,price + 10  new price **from** product;

- 运算符

  - 简介

    - 数据库中的表结构确立后，表中的数据代表的意义就已经确定。通过MySQL运算符进行运算，就可以获取到表结构意外的另一种数据
    - 例如，学生表中存在一个birth字段，这个字段表示学生的出生年份。而运用MySQL的算数运算符用当前的年份减学生出生的年份，那么得到的就是这个学生的实际年龄数据
    - MySQL支持4种运算符
      - 算数运算符
      - 比较运算符
      - 逻辑运算符
      - 位运算符

  - 算数运算符

    - | 算数运算符 | 说明               |
      | ---------- | ------------------ |
      | +          | 加法运算           |
      | -          | 减法运算           |
      | *          | 乘法运算           |
      | /或DIV     | 除法运算，返回商   |
      | %或MOD     | 求余运算，返回余数 |

  - 比较运算符

    - | 比较运算符        | 说明                                                         |
      | ----------------- | ------------------------------------------------------------ |
      | =                 | 等于                                                         |
      | < 和 <=           | 小于和小于等于                                               |
      | > 和 >=           | 大于和大于等于                                               |
      | <=>               | 安全的等于，两个操作码均为NULL时，其所得值为1；而当一个操作码为NULL时，其所得值为0 |
      | <> 或 !=          | 不等于                                                       |
      | IS NULL 或 ISNULL | 判断一个值是否为NULL                                         |
      | IS NOT NULL       | 判断一个值是否不为NULL                                       |
      | LEAST             | 当有两个或多个参数时，返回最小值                             |
      | GREATEST          | 当有两个或多个参数时，返回最大值                             |
      | BETWEEN AND       | 判断一个值是否落在两个值之间                                 |
      | IN                | 判断一个值是IN列表中的任意一个值                             |
      | NOT IN            | 判断一个值不是IN列表中的任意一个值                           |
      | LIKE              | 通配符匹配                                                   |
      | REGEXP            | 正则表达式匹配                                               |

  - 逻辑运算符

    - | 逻辑运算符   | 说明     |
      | ------------ | -------- |
      | NOT 或者 ！  | 逻辑非   |
      | AND 或者 &&  | 逻辑与   |
      | OR 或者 \|\| | 逻辑或   |
      | XOR          | 逻辑异或 |

  - 位运算符

    - | 位运算符 | 说明                               |
      | -------- | ---------------------------------- |
      | \|       | 按位或（有1为1，无1为0）           |
      | &        | 按位与（有0为0，无0为1）           |
      | ^        | 按位异或（相同为0，不同为1）       |
      | <<       | 按位左移（向左移1位，右边补0）     |
      | >>       | 按位右移（向右移1位，左边补0）     |
      | ~        | 按位取反，反转所有比特（32位取反） |

    - 位运算符是在二进制数上进行计算的运算符。位运算会先将操作数变成二进制数，进行位运算。然后再计算结果从二进制数变回十进制数

      

  - 运算符操作

    - 算数运算符查询

      ```sql
      select 6+2;
      select 6-2;
      select 6*2;
      select 6/2;
      select 6%2;
      
      -- 将每件商品的价格加10
      select name,price + 10 as new_price from product;
      -- 将每件商品的价格上调10%
      select pname,price * 1.1 as new_price from product;
      ```

    - 条件查询（通过比较运算符和逻辑运算符）

      ```sql
      -- 查询商品名称为“海尔洗衣机”的商品所有信息:
      select * from product where pname = '海尔洗衣机' ;
      -- 查询价格为800商品 
      select * from product where price = 800;
      -- 查询价格不是800的所有商品
      select * from product where price != 800;   
      select * from product where price <> 800;
      select * from product where not (price = 800) ;
      -- 查询商品价格大于60元的所有商品信息
      select * from product where price > 60;
      -- 查询商品价格在200到1000之间所有商品
      select * from product where price >= 200 and price <= 1000;
      select * from product where price between 200 and 1000;
      select * from product where price >= 200 && price <= 1000;
      -- 查询商品价格是200或800的所有商品
      select * from product where price = 200 or price = 800;
      select * from product where price = 200 || price = 800;
      select * from product where price in (200,800) ;    -- 价格是200或者800
      -- 查询含有'鞋'字的所有商品
      select * from product where pname like '%鞋%' ;   -- 前面加%表示查询'鞋'字在最后的字段，后面加%表示查询'鞋'字在最前的字段，前后都加%表示查询含有'鞋'字的字段（即%用来匹配任意字符）
      -- 查询以'海'开头的所有商品
      select * from product where pname like '海%';
      -- 查询第二个字为，蔻'的所有商品
      select * from product where pname like '_蔻%' ;    -- _下划线匹配单个字符
      -- 查询category_ id为nul1的商品
      select * from product where category_ id is null;
      -- 查询category_ id不为null分类的商品
      select * from product where category_ id is not null;
      -- 使用least求最小值
      select least(10， 20，30) as small_number ; -- 10
      select 1east(10, null，30) ; -- null
      -- 使用greatest求最大值
      select greatest(10， 20，30) as big_number ;   -- 30
      select greatest(10， null，30) ; -- null
      -- 比较时有null结果全为null
      ```



- 排序查询

  - 介绍：

    - 如果我们需要对读取的数据进行排序，我们就可以使用MySQL的**order by** 子句来设定你想按哪个字段那种方式来进行排序，再返回搜索结果。

    - ```sql
      select   
        字段名1，字段名2，……
      from 表名
       order by 字段名1  [asc|desc],字段名2 [asc|desc]……
       
       
       当有多个字段名时，先按照字段1排列，如果字段1相同，再在相同的字段中对字段2进行排列
      ```

  - 特点：

    - asc代表升序，desc代表降序，**如果不写默认升序**
    - order by用于子句中可以支持单个字段，多个字段，表达式，函数，别名
    - order by 子句，放在查询语句的最后面。LIMIT子句除外

  - 操作：

    ```sql
    -- 使用价格排序（降序）
    select * from product order by price desc;
    -- 在价格排序（降序）的基础上，以分类排序（降序）           当price相同时，才排列分类
    select * from product order by price desc,category_id asc;
    -- 显示商品的价格（去重复），并排序
    select distinct price from product order by price desc;
    ```

- 聚合查询

  - 简介：

    - 之前我们做的查询都是横向查询，它们都是根据条件一行一行的进行判断，而使用聚合函数查询是纵向查询，它是对一列的值进行计算，然后返回一个单一的值；另外聚合函数会忽略空值

    - | 聚合函数 | 作用                                                         |
      | -------- | ------------------------------------------------------------ |
      | count()  | 统计指定列不为NULL的记录行数                                 |
      | sum()    | 计算指定列的数值和，如果指定列类型不是数值类型，那么计算结果为0 |
      | max()    | 计算指定列的最大值，如果指定列是字符串类型，那么使用字符串排序运算 |
      | min()    | 计算指定列的最小值，如果指定列是字符串类型，那么使用字符串排序运算 |
      | avg()    | 计算指定列的平均值，如果指定列类型不是数值类型，那么计算结果为0 |

  - 使用：

    ``` sql
    -- 查询商品的总条数
    select count(pid) from product;
    select count(*) from product;
    -- 查询价格大于200商品的总条数
    select count(pid) from product where price > 200;
    -- 查询分类为'c001'的所有商品的总和
    select sum(price) from product where category_id = 'c001';
    --  查询商品的最大价格
    select max(price) from product;
    -- 查询分类为'c002'所有商品的平均价格
    select avg(price) from product where category_id = 'c002';
    ```

  - NULL值的处理

    - count函数对null值的处理

      - 如果count函数的参数为星号（*），则统计所有记录的个数。而如果参数为某字段，不统计含null值的记录个数

    - sum和avg函数对null值的处理

      - 这两个函数忽略null值的存在，就好像该条记录不存在一样

    - max和min函数对null值的处理

      - max和min两个函数同样忽略null值的存在

      

- 分组查询

  - 简介：分组查询是使用**group by** 字句对查询信息进行分组

  - 格式：

    ```sql
    select 字段1，字段2……  from 表名 group by 分组字段 having 分组条件；
    ```

  - 操作：

    ```sql
    -- 统计各个分类商品的个数
    select category_id ,count(*) from product group by category_id;
    ```

  - **如果要进行分组的话，则SELECT子句之后，只能出现分组的字段和统计函数，其他的字段不能出现**

  - 分组之后的条件筛选having

    - **分组之后对统计结果进行筛选的话必须使用having，不能使用where**

    - where子句用来筛选FROM子句中指定的操作所产生的行

    - group by 子句用来分组WHERE子句的输出

    - having子句用来从分组的结果中筛选行

    - 格式：

      ```sql
      select 字段1，字段2……form 表名 group by 分组字段 having 分组条件；
      ```

    - 操作：

      ```sql
      -- 统计各个分类商品的个数，且只显示个数大于4的信息
      select category_id ,count(*) from product group by category_id having count(*) > 4;
      ```

      

- 分页查询**（limit）**

  - 简介：分页查询在项目开发中常见，由于数据量很大，显示屏长度有限，因此对数据需要采取分页显示方式。例如数据共有30条，每页显示5条，第一页显示1--5条，第二页 显示6--10条。

  - 格式：

    ```sql
    -- 方式1-显示前n条
    select 字段1，字段2…… from 表名 limit n
    -- 方式2-分页显示
    select 字段1，字段2…… from 表名 limit m，n
    m:整数，表示从第几条索引开始，计算方式（当前页-1）*每页显示条数
    n：整数，表示查询多少条数据
    
    例子：
    -- 查询product表的前5条记录
    select * from product limit 5 
    -- 从第4条开始显示，显示5条
    select * from product limit 3,5
    ```

    

- INSERT INTO SELECT 语句

  - 简介：将一张表的数据导入到另一张表中，可以使用INSERT INTO SELECT 语句

  - 格式：

    ```sql
    insert into Table2 (field1,field2,……)  select value1,value2,… from Table1
    或者：
    insert into Table2 select * from Table1
    
    例子：
    select into product2(pname,price) select pname,price from product;
    ```

  - **要求目标表Table2必须存在**

- SQL书写顺序

  - ```sql
    select
      category_id,count(pid) cnt
    from
      product
    where
      price > 1000
    group by 
      category_id
    having
      cnt > 4
    order by
      cnt
    limit 1;
    ```

  - select--->where--->group by--->count(pid)--->having--->select--->order by--->limit

- 正则表达式

  - 介绍

    - 正则表达式(regular expression)描述了一种字符串匹配的规则，正则表达式本身就是一个字符串, 使用这个字符串来描述、用来定义匹配规则，匹配一系列符合某个句法规则的字符串。在开发中，正则表达式通常被用来检索、替换那些符合某个规则的文本。
    - MySQL通过**REGEXP关键字**支持正则表达式进行字符串匹配。

  - 格式：

    | 模式       | 描述                                                         |
    | ---------- | ------------------------------------------------------------ |
    | ^          | 匹配输入字符串的开始位置                                     |
    | $          | 匹配输入字符串的结束位置                                     |
    | .          | 匹配除'\n'之外的任何单个字符                                 |
    | [...]      | 字符集合。匹配所包含的任意一个字符。例如：'[abc]'可以匹配"plain"中的'a' |
    | [^...]     | 负值字符集合。匹配未包含的任意字符。例如，[^abc ]可以匹配"plain"中的'p' |
    | p1\|p2\|p3 | 匹配p1或p2或p3。例如：'z\|food'能匹配“z”或者“food”。’（z\|f）ood‘则匹配’zood‘或者’food‘ |
    | *          | 匹配前面的子表达式零次或多次。例如，zo*能匹配“z”以及“zoo”。    *等价于{0，} |
    | +          | 匹配前面的子表达式一次或多次。例如，’zo+‘能匹配“zo”以及“zoo”，但不能匹配“z”。+等价于{1，} |
    | {n}        | n是一个非负整数。匹配确定的n次。例如，’o{2}'不能匹配"Bob"中的‘o'，但是能匹配“food”中的两个o |
    | {n，m}     | m和n均为非负整数，其中n<=m。最少匹配n次且最多匹配m次         |

  - 操作：

    ```sql
    -- ^在字符串开始处进行匹配
    SELECT 'abc' REGEXP '^a';     -- 1(真)
    select * from product where pname regexp '^海';
    
    -- $在字符串末尾开始匹配
    SELECT 'abc' REGEXP 'a$';      -- 0
    SELECT 'abc' REGEXP 'c$';      -- 1
    select * from product where pname regexp '水$';
    
    --  匹配任意单个字符（可以匹配除了换行符外的任意字符）
    SELECT 'abc' REGEXP '.b';    -- 1
    SELECT 'abc' REGEXP '.c';    -- 1
    SELECT 'abc' REGEXP 'a.';    -- 1
    
    -- [...] 匹配括号内的任意单个字符 （正则表达式的任意字符是否在前边的字符串中出现）
    SELECT 'abc' REGEXP '[xyz]';    -- 0
    SELECT 'abc' REGEXP '[xaz]';    -- 1
    
    -- [^...]注意^符合只有在[]内才是取反的意思，在别的地方都是表示开始处匹配
    SELECT 'a' REGEXP '[^abc]';     -- 0
    SELECT 'x' REGEXP '[^abc]';     -- 1
    SELECT 'abc' REGEXP '[^a]';     -- 1
    
    -- a*匹配0个或多个a，包括空字符串。可以作为占位符使用，有没有指定字符都可以匹配到数据
    SELECT 'stab' REGEXP '.ta*b';    -- 1
    SELECT 'stb' REGEXP '.ta*b';    -- 1
    SELECT '' REGEXP 'a*';    -- 1
    
    -- a+ 匹配1个或者多个a，但是不包括空字符
    SELECT 'stab' REGEXP '.ta+b';    -- 1
    SELECT 'stb' REGEXP '.ta+b';    -- 0
    
    -- a? 匹配0个或者1个a 
    SELECT 'stb' REGEXP '.ta?b';    -- 1
    SELECT 'stab' REGEXP '.ta?b';    -- 1
    SELECT 'staab' REGEXP '.ta?b';    -- 0
    
    -- a1|a2 匹配a1或者a2
    SELECT 'a' REGEXP 'a|b';     -- 1
    SELECT 'b' REGEXP 'a|b';     -- 1
    SELECT 'b' REGEXP '^(a|b)';     -- 1
    SELECT 'a' REGEXP '^(a|b)';     -- 1
    SELECT 'c' REGEXP '^(a|b)';     -- 0
    
    -- a{m} 匹配m个a
    SELECT 'auuuuc' REGEXP 'au{4}c';    -- 1
    SELECT 'auuuuc' REGEXP 'au{3}c';    -- 0
    
    -- a{m,} 匹配m个或更多个a
    SELECT 'auuuuc' REGEXP 'au{3，}c';    -- 1
    SELECT 'auuuuc' REGEXP 'au{4，}c';    -- 1
    SELECT 'auuuuc' REGEXP 'au{5，}c';    -- 0
    
    -- a{m,n} 匹配m到n个a，包含m和n
    SELECT 'auuuuc' REGEXP 'au{3，5}c';    -- 1
    SELECT 'auuuuc' REGEXP 'au{4，5}c';    -- 1
    SELECT 'auuuuc' REGEXP 'au{5，10}c';    -- 0
    
    -- (abc)
    -- abc作为一个序列匹配，不用括号括起来都是用单个字符去匹配，如果要把多个字符作为一个整体去匹配就需要用到括号，所以括号适合上面的所有情况
    SELECT 'xababy' REGEXP 'x(abab)y';    -- 1
    SELECT 'xababy' REGEXP 'x(ab)*y';    -- 1
    SELECT 'xababy' REGEXP 'x(ab){1,2}y';    -- 1
    SELECT 'xababy' REGEXP 'x(ab){3}y';    -- 0
    ```

  - **正则表达式匹配的时候要写在where中**







#### MySQL的多表操作

- 多表关系
  - 介绍：实际开发中，一个项目通常需要很多张表才能完成。例如：一个商城项目就需要分类表（catagory）、商品表（products）、订单表（orders）等多张表。且这些表的数据之间存在一定的关系，接下来我们将在单表的基础上，一起学习多表方面的知识。
  - MySQL多表之间的关系可以概括为：**一对一、一对多/多对一关系、多对多**
  - 一对一关系
    - 一个学生只有一张身份证；一张身份证只能对应一学生
    - 在任一表中添加唯一外键。指向另一方主键，确保一对一关系
    - 一般一对一关系很少见，遇到一对一关系的表最好是合并表
    - ![QQ图片20220117194500](C:\Users\ZZY\Desktop\学习资料\markdown插图\QQ图片20220117194500.png)
  - 一对多/多对一关系
    - 部门和员工
    - 分析：一个部门有多个员工，一个员工只能对应一个部门
    - 实现原则：在多的一方建立外键，指向一的一方的主键
    - ![QQ图片20220117194740](C:\Users\ZZY\Desktop\学习资料\markdown插图\QQ图片20220117194740.png)
  - 多对多关系
    - 学生和课程
    - 分析：一个学生可以选择很多门课程，一个课程也可以被很多学生选择
    - 原则：多对多关系实现需要借助第三张中间表。中间表至少包含两个字段，将多对多的关系，拆成一对多的关系，中间至少要有两个外键，这两个外键分别指向原来的那两张表的主键
    - ![QQ图片20220117195707](C:\Users\ZZY\Desktop\学习资料\markdown插图\QQ图片20220117195707.png)

- 外键约束

  - 介绍：

    - MySQL外键约束**(FOREIGN KEY)**是表的一个特殊字段,经常与主键约束一起使用。 对于两个具有关联关系的表而言，相关联字段中主键所在的表就是主表(父表)，外键所在的表就是从表(子表)。
    - 外键用来建立主表与从表的关联关系,为两个表的数据建立连接,约束两个表中数据的一致性和完整性。比如，-个水果摊，只有苹果、桃子、李子、西瓜等4种水果，那么，你来到水果摊要买水果就只能选择苹果、桃子、李子和西瓜，其它的水果都是不能购买的。
    - ![QQ图片20220117200039](C:\Users\ZZY\Desktop\学习资料\markdown插图\QQ图片20220117200039.png)

  - 特点：**定义一个外键时，需要遵循以下规则：**

    - 主表必须已经存在于数据库中,或者是当前正在创建的表。
    - 必须为主表定义主键。
    - **主键不能包含空值，但允许在外键中出现空值**。也就是说，只要外键的每个非空值出现在指定的主键中，这个外键的内容就是正确的。
    - 在主表的表名后面指定列名或列名的组合。这个列或列的组合必须是主表的主键或候选键。
    - 外键中列的数目必须和主表的主键中列的数目相同。
    - 外键中列的数据类型必须和主表主键中对应列的数据类型相同。

  - 操作-创建外键约束

    - 方式1：在创建表时设置外键约束

      - 在create table  语句中，通过**foreign key**关键字来指定外键，具体的语法格式如下：

        ``` SQL
        [constraint<外键名>] foreign key 字段名 [，字段名2，…] references <主表名> 主键列1[,主键列2，…]
        
        例子：
        create database mydb3;
        use mydb3;
        -- 创建部门表
        create table if not exists dept(
            detpno varchar(20) primary key,    -- 部门号
            name varchar(20)  -- 部门名字
        );
        -- 创建员工表（从表）
        create table if not exises emp(
            eid varchar(20)  primary key , -- 员工编号
            ename varchar(20),    -- 员工名字
            age int,    --员工年龄
            dept_id varchar(20),   -- 员工所属部门
            constraint emp_fk foreign key (dept_id) references dept (detpno)  -- 外键约束         
        );
        ```

        

    - 方式2：在创建表后设置外键约束

      - 外键约束也可以在修改表时添加，但是添加外键约束的前提是：从表中外键列中的数据必须与主表中主键列中的数据一致或者是没有数据

      - ```sql
        alter table <数据表名> add constraint <外键名> foreign key (<列名>) references
        
        例子：
        create database mydb3;
        use mydb3;
        -- 创建部门表
        create table if not exists dept2(
            detpno varchar(20) primary key,    -- 部门号
            name varchar(20)  -- 部门名字
        );
        -- 创建员工表（从表）
        create table if not exises emp2(
            eid varchar(20)  primary key , -- 员工编号
            ename varchar(20),    -- 员工名字
            age int,    --员工年龄
            dept_id varchar(20),   -- 员工所属部门      
        );
        alter table emp2 add constraint dep_id_fk foreign key (dept_id) references dept2 (detpno);
        ```

  - 操作-在外键约束下的数据操作

    - 数据插入

      ```sql
      -- 1.添加主表数据
      -- 注意必须献给主表添加数据
      insert into dept values('1001','研发部');
      insert into dept values('1002','销售部');
      insert into dept values('1003','财务部');
      insert into dept values('1004','人事部');
      
      -- 2.添加从表数据
      -- 注意给从表添加数据时，外键列的值不能随便写，必须依赖主表的主键列
      insert into emp values('1','乔峰'，20，'1001');
      insert into emp values('2' ，'段誉' ,21，' 1001') ;
      insert into emp values('3', '虛竹' ,23，'1001');
      insert into emp values('4', '阿紫',18，'1002') ;
      insert into emp values('5', '扫地僧' ,35，' 1002') ;
      insert into emp values('6', '李秋水' ,33，'1003') ;
      insert into emp values('7', '鸠摩智',50，'1003') ;
      insert into emp values('8','天山童姥' ,60，'1005') ;      -- 不可以
      
      ```

    - 删除数据

      ```sql
      -- 3.删除数据
       /* 
       注意：
             1：主表的数据被从表依赖时，不能删除，否则可以删除
             2：从表的数据可以随便删除
       */
       delete from dept where deptno = '1001';      -- 不可以删除
       delete from dept where deptno = '1004';      -- 可以删除
       delete from emp where eid = '7';        -- 可以删除
      ```

      

  - 操作-删除外键约束

    - 当一个表不需要外键约束时，就需要从表中将其删除。外键一旦删除，就会解除主表和从表间的关联关系

    - 格式：

      ```sql
      alter table <表名> drop foreign key  <外键约束名>;
      
      例子：
      alter table mep2 drop foreign key dept_id_fk;
      ```

      

  - 外键约束-多对多关系

    - 在多对多关系中，A表的一行对应B表的多行，B表的一行对应A表的多行，我们要新增加一个中间表，来建立多对多关系

    - ![QQ图片20220117195707](C:\Users\ZZY\Desktop\学习资料\markdown插图\QQ图片20220117195707.png)

    - 操作：

      ```sql
      -- 学生表和课程表（多对多）
      -- 1.创建学生表student (左侧主表)
      create table if not exists student(
          sid int primary key auto_ increment,
          name varchar(20) ，
          age int,
          gender varchar (20)
      );
      -- 2.创建课程表course (右侧主表)
      create table course (
          cid int primary key auto_ increment,
          cidname varchar (20)
      );
      -- 3.创建中间表student_ course/ score (从表)
      create table score(
          sid int,
          cid int,
          score double
      );
      -- 4.建立外键约束(2次)
      alter table score add foreign key (sid) references student (sid) ;
      alter table score add foreign key (cid) references course (cid) ;
      -- 5.给学生表添加数据
      insert into student values(1, '小龙女',18,女')，(2, '阿紫',19,'女'),(3, '周芷若' ,20, '男) ;
      -- 6.给课程表添加数据
      insert into course values(1, '语文')，(2, '数学'),(3, '英语') ;
      -- 7给中间表添加数据
      insert into score values(1,1,78), (1,2,75),(2,1,88)，(2,3,90), (3,2,80), (3,3,65) ;
          
      -- 修改和删除时，中间从表可以随便删除和修改，但是两边的主表受从表依赖的数据不能删除或者修改
      ```

      

- 多表联合查询

  - 介绍：多表查询就是同时查询两个或两个以上的表，因为有的时候用户在查看数据的时候，需要显示的数据来自多张表。多表查询有以下分类：

    - 交叉连接查询[产生笛卡尔积，了解]

      语法: select * from A,B;

    - 内连接查询(使用的关键字inner join - inner可以省略)

      隐式内连接(SQL92标准) : select * from A,B where条件;

      显示内连接(SQL99标准) : select * from A inner join B on 条件;

    - 外连接查询(使用的关键字outer join - outer可以省略)

      左外连接: left outer join

      select * from A left outer join B on 条件;

      右外连接: right outer join

      select * from A right outer join B on 条件;

      满外连接: full outer join

      select * from A full outer join B on条件;

    - 子查询：

      select的嵌套

    - 表自关联：

      将一张表当成多张表来用

    - ![QQ图片20220117213411](C:\Users\ZZY\Desktop\study\markdown插图\QQ图片20220117213411.png)

  - 准备查询数据

    - 接下来准备多表查询需要的数据，注意，**外键约束对多表查询并无影响。**

    - ```sql
      -- 创建部门表
      create table if not exists dept3 (
          deptno varchar(20) primary key，-- 部门号
          name varchar(20) -- 部门名字
      );
      
      -- 创建员工表
      create table if not exists emp3 (
          eid varchar(20) primary key，-- 员工编号
          ename varchar(20)， -- 员工名字
          age int, -- 员工年龄
          dept_id varchar(20) -- 员工所属部门
      );
      -- 给dept3表添加数据
      insert into dept3 values('1001','研发部');
      insert into dept3 values('1002','销售部');
      insert into dept3 values('1003','财务部');
      insert into dept3 values('1004','人事部');
      -- 给emp3表添加数据
      insert into emp3 values('1', '乔峰' ,20，'1001') ;
      insert into emp3 values('2', '段誉' ,21，'1001') ;
      insert into emp3 values('3', '虚竹' ,23，'1001') ;
      insert into emp3 values('4', '阿紫' ,18，'1001') ;
      insert into emp3 values('5', '扫地僧'，,85，'1002');
      insert into emp3 values('6', '李秋水' ,33，'1002');
      insert into emp3 values('7', '鸠摩智' ,50，'1002');
      insert into emp3 values('8','天山童姥' ,60，'1003');
      insert into emp3 values('9', '慕容博' ,58，'1003');
      insert into emp3 values('10','丁春秋' ,71，'1005');
      
      ```

      

  - 交叉连接查询

    - 交叉连接查询返回被连接的两个表所有数据行的笛卡尔积

    - 笛卡尔积可以理解为一张表的每一行去和另外一张表的任意一行进行匹配

    - 假如A表有m行数据，B表有n行数据，则返回m*n行数据

    - 笛卡尔积会产生很多冗余的数据，后期的其他查询可以在该集合的基础上进行条件筛选

    - 格式：

      ``` sql
      select * from 表1，表2，表3……；
      
      例子：
      -- 交叉连接查询-
      select * from dept3,emp3;
      ```

  - 内连接查询

    - 内连接查询求多张表的交集

    - 格式：

      ```sql
      隐式内连接(SQL92标准) : select * from A,B where条件;
      
      显示内连接(SQL99标准) : select * from A inner join B on 条件;
      ```

    - 操作：

      ```sql
      -- 查询每个部门的所属员工
      select * from dept3,emp3 where dept3.deptno = emp3.dept_id;
      select * from dept3 inner join emp3 on dept3.deptno = emp3.dept_id;
      -- 查询研发部和销售部的所属员工
      select * from dept3, ehp3 where dept3. deptno = emp3.dept_ idand name in('研发部', '销售部') ;
      select * from dept3 join emp3 on dept3.deptno = emp3.dept_ idand name in( '研发部', '销售部') ;
      -- 查询每个部门的员工数,并升序排序
      select deptno, count(1) as total_ cnt from dept3,emp3 where dept3.deptno = emp3.dept_id group by deptno order by total_cnt;
      select deptno, count(1) as total_ cnt from dept3 join emp3 on dept3.deptno = emp3.dept_id group by deptno order by total_cnt;
      -- 查询人数大于等于3的部门，并按照人数降序排序
      select deptno, count(1) as total_cnt from dept3,emp3 where
      dept3.deptno = emp3.dept_id group by deptno having total_cnt >= 3 order by total_cnt desc;
      
      select deptno, count(l) as total_cnt from dept3 join emp3 on dept3.deptno = emp3.dept_id group by deptno having total_cnt >= 3 order by total_cnt desc;
      
      ```

      

  - 外连接查询

    - 外连接分为左外连接（left outer join）、右外连接（right outer join）、满外连接（full outer join）。

    - **注意：oracle里面有full join，可是在MySQL对full join支持的不好。我们可以用union来达到目的**

    - 格式：

      ```sql
      左外连接: left outer join
      select * from A left outer join B on 条件;
      右外连接: right outer join
      select * from A right outer join B on 条件;
      满外连接: full outer join
      select * from A full outer join B on条件;
      ```

    - 操作：

      ```sql
      -- 外连接查询
      -- 查询哪些部门有员工，哪些部门没有员工
      use mydb3;
      select * from dept3 left outer join emp3 on dept3.deptno = emp3.dept_id;
      select * from dept3 left join emp3 on dept3.deptno = emp3.dept_id;
      
      select * from A left join B on 条件1 left join C on 条件2
      -- 查询员工有对应的部门，哪些没有
      select * from dept3 right outer join emp3 on dept3.deptno = emp3.dept_id;
      select * from dept3 right join emp3 on dept3.deptno = emp3.dept_id;
      
      select * from A right join B on 条件1 right join C on 条件2
      -- 使用union关键字实现左外连接和右外连接的并集
      -- union是将两个查询结果上下拼接，并去重（满外连接）
      select * from dept3 left outer join emp3 on dept3.deptno = emp3.dept_id
      union
      select * from dept3 right outer join emp3 on dept3.deptno = emp3.dept_id;
      -- union all 是将两个查询结果上下拼接，不去重
      select * from dept3 left outer join emp3 on dept3.deptno = emp3.dept_id
      union all
      select * from dept3 right outer join emp3 on dept3.deptno = emp3.dept_id;
      ```

      

  - 子查询

    - 介绍：子查询就是指的在一个完整的查询语句之中，嵌套若干个不同功能的小查询，从而一起完成复杂查询的一种编写形式，通俗一点就是包含**select嵌套的查询**

    - 特点：子查询可以返回的数据类型一共分为四种：

      - 1.单行单列：返回的是一个具体列的内容，可以理解为一个单值数据；
      - 2.单行多列：返回一行数据中多个列的内容
      - 3.多行单列：返回多行记录中同一列的内容，相当于给出了一个操作范围
      - 4.多行多列：查询返回的结果是一张临时表

    - 操作：

      ```sql
      -- 查询年龄最大的员工信息，显示信息包括员工号，员工名字，员工年龄
      -- 1.查询最大年龄
        select max(age) from emp3;
      -- 2.让每一个员工的年龄和最大年龄进行比较，相等则满足条件
      select * from emp3 where age = (select max(age) from emp3);    -- 单行单列，可以作为一个值来用
      
      -- 查询研发部和销售部的员工信息，包括员工号，员工名字
      -- 方式1：关联查询
      select * from dept3 a join emp3 b on a.deptno = b.dept_id and (name = '研发部' or name = '销售部');
      -- 方式2：子查询
      -- 2.1 先查询研发部和销售部的部门号：deptno
      select deptno from dep3 where name = '研发部' or name = '销售部';
      -- 2.2 查询哪个员工的部门号是1001或1002
      select * from emp3 where dept_id in (select deptno from dep3 where name = '研发部' or name = '销售部');      -- 单列多行，多个值
      
      -- 查询研发部20岁以下的员工信息，包括员工号，员工名字，部门名字
      -- 方式1-关联查询
      select * from dept3 a join emp3 b on a.deptno = b.dept_id and (name = '研发部' and age < 20);
      -- 方式2-子查询
      -- 2.1 在部门表中查询研发部信息
      select * from dept3 where name = '研发部';
      -- 2.2 在员工表中查询年龄小于20岁的员工信息
      select * from emp3 where age < 20;
      -- 2.3 将以上两个查询的结果进行关联查询
      select * from (select * from dept3 where name = '研发部') t1 join (select * from emp3 where age < 20) t2 on t1.deptno = t2.dept_id;           -- 多行多列
      ```

    - 子查询关键字

      在子查询中，有一些常用的逻辑关键字，这些关键字可以给我们提供更丰富的查询功能，主要关键字如下：

      - ALL关键字
      - ANY关键字
      - SOME关键字
      - IN关键字
      - EXISTS关键字

    - 子查询关键字-ALL

      - 格式：

        ```sql
        select ...from ...where c > all (查询语句)
        -- 等价于:
        select ... from ... where c > resultl and C > result2 and c > result3
        ```

      - 特点：

        - ALL:与子查询返回的所有值比较为true则返回true
        - ALL可以与=、>、>=、<、<=、<>结合是来使用，分别表示等于、大于、大于等于、小于、小于等于、不等于其中的其中的所有数据。
        - ALL表示指定列中的值必须要大于子查询集的每一个值， 即必须要大于子查询集的最大值;如果是小于号即小于子查询集的最小值。同理可以推出其它的比较运算符的情况。

      - 操作：

        ```sql
        -- 查询年龄大于'1003'部门所有年龄的员工信息
        select * from emp3 where age > all (select age from emp3 where dept_id = '1003');
        -- 查询不属于任何一个部门的员工信息
        select * from emp3 where dept_id != all(select deptno from dept3);
        ```

    - 子查询关键字-ANY和SOME 

      - 格式：

        ```sql
        select ... from ... where c > any(查询语句)
        -- 等价于:
        select...from ... where c > resultl or C > result2 or C > result3
        ```

      - 特点：

        - ANY:与子查询返回的任何值比较为true则返回true
        - ANY可以与=、>、>=、<、<=、<>结合是来使用，分别表示等于、大于、大于等于、小于、小于等于、不等于其中的其中的任何一一个数据。
        - 表示制定列中的值要大于子查询中的任意一个值， 即必须要大于子查询集中的最小值。同理可以推出其它的比较运算符的情况。.
        - **SOME和ANY的作用一样，SOME 可以理解为ANY的别名**

      - 操作：

        ```sql
        -- 查询年龄大于'1003'部门任意一个员工年龄的员工信息
        select * from emp3 where age > a11 (select age from emp3 where dept_id = '1003') ;
        ```

    - 子查询关键字-IN

      - 格式：

        ```sql
        select ... from ... where C in(查询语句)
        -- 等价于：
        select ... from ..where C = result1 or c = result2 or c = result3
        ```

      - 特点：

        - IN关键字，用于判断某个记录的值，是否在指定的集合中
        - 在IN关键字前边加上not可以将条件反过来

      - 操作：

        ```sql
        -- 查询研发部和销售部的员工信息，包含员工号、员工名字
        select eid,ename,t.name from emp3 where dept_id in (select deptno from dept3 where name = '研发部' or name = '销售部');
        ```

    - 子查询关键字-EXISTS

      - 格式：

        ```sql
        select ... from ... where exists (查询语句)
        ```

      - 特点：

        - 该子查询如果“有数据结果”(至少返回一行数据)，则该EXISTS() 的结果为“true", 外层查询执行
        - 该子查询如果“没有数据结果”(没有任何数据返回) ，则该EXISTS()的结果为“false", 外层查询不执行
        - EXISTS后面的子查询不返回任何实际数据，只返回真或假，当返回真时where条件成立
        - 注意，EXISTS关键字，比IN关键字的运算效率高，因此，在实际开发中，特别是大数据量时，推荐使用EXISTS关键字

      - 操作：

        ```sql
        -- 查询公司是否有大于60岁的员工，有则输出
        select * from emp3 a where exists(select * from emp3 b where a.age > 60);
        select * from emp3 a where eid in (select * from emp3 b where a.age > 60);
        -- 查询有所属部门的员工信息
        select * from dept3 a where exists (select * from emp3 b where a.deptno = b.dept_id);
        select * from dept3 a where dept_id in (select * from emp3 b where a.deptno = b.dept_id);
        ```

        

  - 自关联查询

    - 概念：MySQL有时在信息查询时需要进行对表自身进行关联查询，即一张表自己和自己关联，一张表当成多张表来用。注意**自关联时表必须给表起别名。**

    - 格式：

      ```sql
      select 字段列表 from 表1 a，表1 b where 条件;
      或者
      select 字段列表 from 表1 a [left] join 表1 b on条件;
      ```

    - 操作：

      ```sql
      -- 创建表，并建立自关联约束
      create table t_sanguo(
      eid int primary key ,    -- 主键列
      ename varchar (20) ,
      manager_id int,      -- 外键列
      foreign key (manager_id) references t_sanguo (eid)    -- 添加自关联约束
      );
      -- 添加数据
      insert into t_sanguo values(1, '刘协' ,NULL) ;
      insert into t_sanguo values(2, '刘备',1) ;
      insert into t_sanguo values(3, '关羽',2) ;
      insert into t_sanguo values(4, '张飞',2) ;
      insert into t_sanguo values(5, '曹操' ,1) ;
      insert into t_sanguo values(6, '许褚',5) ;
      insert into t_sanguo values(7, '典韦' ,5) ;
      insert into t_sanguo values(8, '孙权',1) ;
      insert into t_sanguo values(9, '周瑜' ,8) ;
      insert into t_sanguo values(10, '鲁肃' ,8) ;
      -- 进行关联查询
      -- 1.查询每个三国人物及他的上级信息，如:关羽刘备
      select * from t_sanguo a, t_sanguo b where a.manager_id = b.eid;
      select a.ename,b.ename from t_sanguo a join t_sanguo b on a.manager_id = b.eid;
      -- 2.查询所有任务及上级
      select a.ename,b.ename from t_sanguo a left join t_sanguo b on a.manager_id = b.eid;
      -- 3.查询所有人物、上级、上上级，比如：张飞 刘备 刘协
      select 
      a.ename,b.ename 
      from t_sanguo a 
      left join t_sanguo b on a.manager_id = b.eid
      left join t_sanguo c on b.manager_id = c.eid;
      ```

