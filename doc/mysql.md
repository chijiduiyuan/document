### 数据库三大范式
* 第一范式：每个列都不可以再拆分。
* 第二范式：在第一范式的基础上，非主键列完全依赖于主键，而不能是依赖于主键的一部分。
* 第三范式：在第二范式的基础上，非主键列只依赖于主键，不依赖于其他非主键。

### mysql有关权限的表都有哪几个
MySQL服务器通过权限表来控制用户对数据库的访问，权限表存放在mysql数据库里，由mysql_install_db脚本初始化。这些权限表分别user，db，table_priv，columns_priv和host。下面分别介绍一下这些表的结构和内容：

* user权限表：记录允许连接到服务器的用户帐号信息，里面的权限是全局级的。
* db权限表：记录各个帐号在各个数据库上的操作权限。
* table_priv权限表：记录数据表级的操作权限。
* columns_priv权限表：记录数据列级的操作权限。
* host权限表：配合db权限表对给定主机上数据库级操作权限作更细致的控制。这个权限表不受GRANT和REVOKE语句的影响。

### MySQL的binlog有几种录入格式？分别有什么区别？
有三种格式，statement，row和mixed。

* statement模式下，每一条会修改数据的sql都会记录在binlog中。不需要记录每一行的变化，减少了binlog日志量，节约了IO，提高性能。由于sql的执行是有上下文的，因此在保存的时候需要保存相关的信息，同时还有一些使用了函数之类的语句无法被记录复制。
* row级别下，不记录sql语句上下文相关信息，仅保存哪条记录被修改。记录单元为每一行的改动，基本是可以全部记下来但是由于很多操作，会导致大量行的改动(比如alter table)，因此这种模式的文件保存的信息太多，日志量太大。
* mixed，一种折中的方案，普通操作使用statement记录，当无法使用statement的时候使用row。
此外，新版的MySQL中对row级别也做了一些优化，当表结构发生变化的时候，会记录语句而不是逐行记录。

### 数据类型

| 分类 | 类型名称 | 说明 | 
| :---- | :---- | :----- | 
| 整数类型 | tinyint | 1字节，范围-128-127 | 
|        | smallint | 2字节 | 
|        | mediumint | 3字节 | 
|        | int | 4字节 | 
|        | bigint | 8字节 | 
| 小数类型 | float | 单精度浮点数 | 
|        | double | 双精度浮点数 | 
|        | decimal(m,d) | 压缩严格的定点数 | 
| 日期类型 | year | YYYY 1901-2155 | 
|        | time | HH:MM:SS -838:59:59-838:59:59 | 
|        | date | YYYY-MM-DD 1000-01-01~9999-12-3 | 
|        | datetime | YYYY-MM-DD HH:MM:SS 1000-01-01 00:00:00~ 9999-12-31 23:59:59 | 
|        | timestamp | YYYY-MM-DD HH:MM:SS 19700101 00:00:01 UTC~2038-01-19 03:14:07UTC | 
| 文本、二进制类型 | CHAR(M) | M(0-255之间的整数) | 
|           | VARCHAR(M) | M(0-65535之间的整数) | 
|           | TINYBLOB | 0-255字节 | 
|           | BLOB | 0-65535字节 | 
|           | MEDIUMBLOB | 0-16772150字节 | 
|           | LONGBLOB | 0-4294967295字节 | 
|           | TINYTEXT | 0-255字节 | 
|           | TEXT | 0-65535字节 | 
|           | MEDIUMTEXT | 0-167772150字节 | 
|           | LONGTEXT | 0-4294967295字节 | 
|           | VARBINARY(M) | 允许长度0~M个字节的变长字节字符串 | 
|           | BINARY(M) | 允许长度0~M个字节的定长字节字符串 | 

* 1.任何整数类型都可以加上UNSIGNED属性，表示数据是无符号的，即非负整数。整数类型可以被指定长度
* 2.DECIMAL可以用于存储比BIGINT还大的整型，能存储精确的小数。
* 3.VARCHAR用于存储可变长字符串，它比定长类型更节省空间。

### 存储引擎
* Innodb引擎：Innodb引擎提供了对数据库ACID事务的支持。并且还提供了行级锁和外键的约束。它的设计的目标就是处理大数据容量的数据库系统。
* MyIASM引擎(原本Mysql的默认引擎)：不提供事务的支持，也不支持行级锁和外键。
* MEMORY引擎：所有的数据都在内存中，数据的处理速度快，但是安全性不高。

### 索引覆盖
* 如果要查询的字段都建立过索引，那么引擎会直接在索引表中查询而不会访问原始数据（否则只要有一个字段没有建立索引就会做全表扫描），这叫索引覆盖。因此我们需要尽可能的在select后只写必要的查询字段，以增加索引覆盖的几率。
* 使用场景 where、order by、join
* 分析 explain select * from tabale1 where name='zs'

### 索引
* 主键索引: 数据列不允许重复，不允许为NULL，一个表只能有一个主键。

* 唯一索引: 数据列不允许重复，允许为NULL值，一个表允许多个列创建唯一索引。

  可以通过 ALTER TABLE table_name ADD UNIQUE (column); 创建唯一索引

  可以通过 ALTER TABLE table_name ADD UNIQUE (column1,column2); 创建唯一组合索引

* 普通索引: 基本的索引类型，没有唯一性的限制，允许为NULL值。

  可以通过ALTER TABLE table_name ADD INDEX index_name (column);创建普通索引

  可以通过ALTER TABLE table_name ADD INDEX index_name(column1, column2, column3);创建组合索引

* 全文索引： 是目前搜索引擎使用的一种关键技术。

  可以通过ALTER TABLE table_name ADD FULLTEXT (column);创建全文索引

### 索引数据结构
* B树

### 锁
* 从锁级别区分
* 表级锁 myisam
* 行级锁 innodb默认
* 页级锁

* 从锁类别区分
* 共享锁  当用户要进行数据的读取时，对数据加上共享锁。共享锁可以同时加上多个。
* 排他锁 当用户要进行数据的写入时，对数据加上排他锁。排他锁只可以加一个，他和其他的排他锁，共享锁都相斥。
例: select * from tab_with_index where id = 1 for update;

### 常用SQL语句
SQL语句主要分为哪几类
数据定义语言DDL（Data Ddefinition Language）CREATE，DROP，ALTER

主要为以上操作 即对逻辑结构等有操作的，其中包括表结构，视图和索引。

数据查询语言DQL（Data Query Language）SELECT

这个较为好理解 即查询操作，以select关键字。各种简单查询，连接查询等 都属于DQL。

数据操纵语言DML（Data Manipulation Language）INSERT，UPDATE，DELETE

主要为以上操作 即对数据进行操作的，对应上面所说的查询操作 DQL与DML共同构建了多数初级程序员常用的增删改查操作。而查询是较为特殊的一种 被划分到DQL中。

数据控制功能DCL（Data Control Language）GRANT，REVOKE，COMMIT，ROLLBACK

主要为以上操作 即对数据库安全性完整性等有操作的，可以简单的理解为权限控制等。


### 超大分页怎么处理？
超大的分页一般从两个方向上来解决.

* 数据库层面,这也是我们主要集中关注的(虽然收效没那么大),类似于select * from table where age > 20 limit 1000000,10这种查询其实也是有可以优化的余地的. 这条语句需要load1000000数据然后基本上全部丢弃,只取10条当然比较慢. 当时我们可以修改为select * from table where id in (select id from table where age > 20 limit 1000000,10).这样虽然也load了一百万的数据,但是由于索引覆盖,要查询的所有字段都在索引中,所以速度会很快. 同时如果ID连续的好,我们还可以select * from table where id > 1000000 limit 10,效率也是不错的,优化的可能性有许多种,但是核心思想都一样,就是减少load的数据.
* 从需求的角度减少这种请求…主要是不做类似的需求(直接跳转到几百万页之后的具体某一页.只允许逐页查看或者按照给定的路线走,这样可预测,可缓存)以及防止ID泄漏且连续被人恶意攻击.

### 开启慢查询日志

* 配置项：slow_query_log

可以使用show variables like ‘slov_query_log’查看是否开启，如果状态值为OFF，可以使用set GLOBAL slow_query_log = on来开启，它会在datadir下产生一个xxx-slow.log的文件。

设置临界时间

* 配置项：long_query_time

查看：show VARIABLES like 'long_query_time'，单位秒

设置：set long_query_time=0.5

实操时应该从长时间设置到短的时间，即将最慢的SQL优化掉

查看日志，一旦SQL超过了我们设置的临界时间就会被记录到xxx-slow.log中


### 读写分离
一. 主从复制概述
在实际生产中，数据的重要性不言而喻，提供安全可靠的数据保障是技术与运维部门的职责所在；如果我们的数据库只有一台服务器，那么很容易产生单点故障的问题，比如这台服务器访问压力过大而没有响应或者奔溃，那么服务就不可用了，再比如这台服务器的硬盘坏了，那么整个数据库的数据就全部丢失了，这是重大的安全事故；为了避免服务的不可用以及保障数据的安全可靠性，我们至少需要部署两台或两台以上服务器来存储数据库数据，也就是我们需要将数据复制多份部署在多台不同的服务器上，即使有一台服务器出现故障了，其他服务器依然可以继续提供服务；主从复制是指服务器分为主服务器和从服务器，主服务器负责读和写，从服务器只负责读，主从复制也叫 master/slave，master是主，slave是从；

二. 主从复制架构
说明:一般情况下,具体架构还得看数据量大小来定,数据量规模较小的情况下,使用一主一从的架构的较多.一主一从的弊端就是容易出现单点故障,一旦主库故障便不能进行写入操作,所以,数据量较大时就需要使用处理高并发的思想来解决问题了,比如:一方面可以做分压处理(Nginx集群,MySql集群等等),一方面可以做异步处理,用时间换空间(ActiveMQ消息队列).MySql高并发的处理方案就是多主多从,可以极大地提高数据库的容灾能力.

三. 主从复制原理
1. 当 master 主服务器上的数据发生改变时，则将其改变写入二进制日志文件中；
2. salve 从服务器会在一定时间间隔内对 master 主服务器上的二进制日志进行探测，探测其是否发生过改变；
3. 如果探测到 master 主服务器的二进制日志发生了改变，则开始一个 I/O Thread 请求 master 二进制事件；
4. 同时 master 主服务器为每个 I/O Thread 启动一个dump Thread，用于向其发送二进制事件；
5. slave 从服务器将接收到的二进制事件保存至自己本地的中继日志文件中；
6. salve 从服务器将启动 SQL Thread 从中继日志中读取二进制日志，在本地重放，使得其数据和主服务器保持一致；
7. 最后 I/O Thread 和 SQL Thread 将进入睡眠状态，等待下一次被唤醒；