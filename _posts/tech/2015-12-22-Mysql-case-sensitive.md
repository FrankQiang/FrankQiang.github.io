---
layout: post
title: Mysql case sensitive analysis
category: 技术
tags: Mysql case sensitive analysis
keywords: Mysql case sensitive analysis
---



#### 什么是字符集和校验规则？

字符集是一套符号和编码。校对规则是在字符集内用于比较字符的一套规则。任何一个给定的字符集至少有一个校对规则，它可能有几个校对规则。要想列出一个字符集的校对规则，使用SHOW COLLATION语句。

![1-600x253](http://www.wocycle.com/uploads/users/blog/2015/12/22/ab4a9aadf7a93c3d06a35316bf59c051_18.png)

校对规则一般有这些特征：

* 两个不同的字符集不能有相同的校对规则。
* 每个字符集有一个默认校对规则。例如，utf8默认校对规则是`utf8_general_ci`。
* 存在校对规则命名约定：它们以其相关的字符集名开始，通常包括一个语言名，并且以`_ci`（大小写不敏感）、`_cs`（大小写敏感）或`_bin`（二元）结束。



#### 不同级别的字符集和校验规则可控制大小写敏感

MySQL5.1在同一台服务器、同一个数据库或甚至在同一个表中使用不同字符集或校对规则来混合定义字符串。字符集和校对规则有4个级别的默认设置：服务器级、数据库级、表级和连接级。

##### 服务器级

MySQL按照如下方法确定服务器字符集和服务器校对规则：

(1)修改配置文件/etc/my.cnf

    在[mysqld]下添加：collation_server = utf8_bin

重启实例

![21](http://www.wocycle.com/uploads/users/blog/2015/12/22/00e10b8f29127665a9bdb4c5ef1cb57d_18.png)

    更改服务器级的校验规则（collation_server ）后，数据库校验规则（collation_collation）默认会继承服务器级的。

注意：

这个只适用于在重新启动之后, 新建的库,已存在的库不受影响.

同样的, 即使库的校验规则改了,已经存在的表不受修改影响;

同理与已经存在的列…

    mysql> create database yutest0;
    Query OK, 1 row affected (0.00 sec)
    mysql> use yutest0;
    Database changed
    mysql> create table t1 (name varchar(10));
    Query OK, 0 rows affected (0.01 sec)

    mysql> insert into t1 values('AAA');
    Query OK, 1 row affected (0.00 sec)
    mysql> insert into t1 values('aaa');
    Query OK, 1 row affected (0.01 sec)

    mysql> select * from t1;
    +------+
    | name |
    +------+
    | AAA  |
    | aaa  |
    +------+
    2 rows in set (0.00 sec)

    mysql> select * from t1 where name='aaa';
    +------+
    | name |
    +------+
    | aaa  |
    +------+
    1 row in set (0.00 sec)

可以看出，在服务器级进行相应的校对规则设置，查询大小写敏感。

(2)当服务器启动时根据有效的选项设置

当启动mysqld时，根据使用的初始选项设置来确定服务器字符集和校对规则。

    shell> mysqld --character-set-server=latin1 --collation-server=latin1_swedish_ci

##### 数据库级

MySQL这样选择数据库字符集和数据库校对规则：

* 如果指定了character set X和collate Y，那么采用字符集X和校对规则Y。
* 如果指定了character set X而没有指定collate Y，那么采用character set X和character set X的默认校对规则。
* 否则，采用服务器字符集和服务器校对规则。

(1)修改配置文件/etc/my.cnf

进行了两组测试：

1) 在[mysqld]下添加：

    collation_server = utf8_bin

    collation_database = utf8_bin

2) 在[mysqld]下添加：

    collation_database = utf8_bin

重启实例，两组都不能正常启动，错误信息如下：

![3-600x48](http://www.wocycle.com/uploads/users/blog/2015/12/22/d7ef2366a7a0947160cf868dce603625_18.png)

可见，my.cnf配置文件中不支持设置collation_database 变量。

（2）创建数据库时设置数据库校验规则

    mysql> create database yutest default character set utf8 collate utf8_bin;
    Query OK, 1 row affected (0.00 sec)
    mysql> show variables like 'collation_%';
    +----------------------+-----------------+
    | Variable_name        | Value           |
    +----------------------+-----------------+
    | collation_connection | utf8_general_ci |
    | collation_database   | utf8_bin        |
    | collation_server     | utf8_general_ci |
    +----------------------+-----------------+
    3 rows in set (0.00 sec)
    mysql> select * from t1;
    +------+
    | name |
    +------+
    | ABC  |
    | abc  |
    +------+
    2 rows in set (0.00 sec)

     mysql> select * from t1 where name='abc';
    +------+
    | name |
    +------+
    | abc  |
    +------+
    1 row in set (0.01 sec)

可以看出，在数据库级进行相应的校对规则设置，查询大小写敏感。



##### 表级

MySQL按照下面的方式选择表字符集和校对规则：

* 如果指定了character set X和collate Y，那么采用character set X和collate Y。
* 如果指定了character set X而没有指定collate Y，那么采用character set X和character set X的默认校对规则。
* 否则，采用数据库字符集和服务器校对规则。

在创建表时设置表级校验规则：

    mysql> create database yutest2;
    Query OK, 1 row affected (0.01 sec)
    mysql> use yutest2;
    Database changed

    mysql> create table t1(name varchar(10))
        -> default character set utf8 collate utf8_bin;
    Query OK, 0 rows affected (0.01 sec)

    mysql> insert into t1 values('ABC');
    Query OK, 1 row affected (0.00 sec)
    mysql> insert into t1 values('abc');
    Query OK, 1 row affected (0.00 sec)

    mysql> show variables like 'collation_%';
    +----------------------+-----------------+
    | Variable_name        | Value           |
    +----------------------+-----------------+
    | collation_connection | utf8_general_ci |
    | collation_database   | utf8_general_ci |
    | collation_server     | utf8_general_ci |
    +----------------------+-----------------+
    3 rows in set (0.00 sec)

    mysql> select * from t1;
    +------+
    | name |
    +------+
    | ABC  |
    | abc  |
    +------+
    2 rows in set (0.00 sec)

    mysql> select * from t1 where name='abc';
    +------+
    | name |
    +------+
    | abc  |
    +------+
    1 row in set (0.00 sec)

可以看出，在表级进行相应的校对规则设置，查询大小写敏感。



##### 连接级

考虑什么是一个“连接”：它是连接服务器时所作的事情。客户端发送SQL语句，例如查询，通过连接发送到服务器。服务器通过连接发送响应给客户端，例如结果集。对于客户端连接，这样会导致一些关于连接的字符集和校对规则的问题，这些问题均能够通过系统变量来解决：

    mysql> show variables like 'character%';
    +--------------------------+----------------------------+
    | Variable_name            | Value                      |
    +--------------------------+----------------------------+
    | character_set_client     | utf8                       |
    | character_set_connection | utf8                       |
    | character_set_database   | utf8                       |
    | character_set_filesystem | binary                     |
    | character_set_results    | utf8                       |
    | character_set_server     | utf8                       |
    | character_set_system     | utf8                       |
    | character_sets_dir       | /usr/share/mysql/charsets/ |
    +--------------------------+----------------------------+
    8 rows in set (0.00 sec)

        当查询离开客户端后，在查询中使用哪种字符集？

    服务器使用character_set_client变量作为客户端发送的查询中使用的字符集。

        服务器接收到查询后应该转换为哪种字符集？

    转换时，服务器使用character_set_connection和collation_connection系统变量。它将客户端发送的查询从character_set_client系统变量转换到character_set_connection。

        服务器发送结果集或返回错误信息到客户端之前应该转换为哪种字符集？

    character_set_results变量指示服务器返回查询结果到客户端使用的字符集。包括结果数据，例如列值和结果元数据（如列名）。

如果需要设置编码 set character_set_client utf8

    set names utf8之前，

    character_set_client | gbk

    character_set_connection| gbk

    character_set_results | gbk

    set names utf8之后，

    character_set_client | utf8

    character_set_connection| utf8

    character_set_results | utf8


#### 创建数据库表时大小写不敏感，仍然有方法在查询时区分大小写

##### 在SQL语句中使用collate

使用collate子句，能够为一个比较覆盖任何默认校对规则。collate可以用于多种SQL语句中，比如where，having，group by，order by，as，聚合函数。

    mysql> select * from t1 where name collate utf8_bin = 'ABC';
    +------+
    | name |
    +------+
    | ABC |
    +------+
    1 row in set (0.00 sec)

    mysql> select * from t1 where name = 'ABC';
    +------+
    | name |
    +------+
    | ABC |
    | Abc |
    | abc |
    +------+
    3 rows in set (0.00 sec)

    mysql> select * from t1;
    +------+
    | name |
    +------+
    | ABC |
    | Abc |
    | abc |
    +------+
    3 rows in set (0.00 sec)

##### binary操作符

binary操作符是collate子句的一个速记符。binary ’x‘等价与’x‘ collate y，这里y是字符集’x‘二元校对规则的名字。每一个字符集有一个二元校对规则。例如，latin1字符集的二元校对规则是latin1_bin，因此，如果列a是字符集latin1，以下两个语句有相同效果：

    select * from t1 order by binary a;

    select * from t1 order by a collate latin1_bin;

    mysql> select * from t1 where binary name = 'ABC';
    +------+
    | name |
    +------+
    | ABC |
    +------+
    1 row in set (0.00 sec)
    mysql>
    mysql> select * from t1 where name = 'ABC';
    +------+
    | name |
    +------+
    | ABC |
    | Abc |
    | abc |
    +------+
    3 rows in set (0.00 sec)



参考链接：

MySQL5.1参考手册 [http://dev.mysql.com/doc/refman/5.1/en/charset-server.html](http://dev.mysql.com/doc/refman/5.1/en/charset-server.html)
