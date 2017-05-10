---
layout: post
title: Mysql笔记
category: 技术
tags: Mysql
keywords: Mysql, 笔记
---

#### 数据管理

**设置root用户**

    mysqladmin -u root password ********

**登录数据库**

    mysql (-h localhost 可选,可以连接远程服务器) -u 用户名 -p

**使用数据库**

    USE DATABASE

**显示所有数据库**

    SHOW DATABASES

**创建数据库**

    CREATE DATABASE database_name

**删除数据库**

    DROP DATABASE database_name

**重命名数据库**

    RENAME DATABASE old_database_name TO  new_database_name

    注意 mysql 没有这个功能


#### 数据类型

    integer(size) int(size) smallint(size) tinyint(size)   整型数据

    decimal(size,d) numeric(size,d)                        浮点数据

    char(size)                                             固定字符串

    varchar(size)                                          可变字符串

    date(yymmdd)                                           日期

    char 与 varchar : char会分配固定长度,varchar会分配小于规定长度的任意长度

    decimal 与 numeric 相同 decimal(10,4)表示的是最大可达十位（包含小数），小数四位


#### 创建表

    CREATE TABLE table_name (

    列名称1 数据类型,

    列名称2 数据类型,

    ...

    );

#### 处理表的命令

**查看表结构**

    DESCRIBE table_name

**删除表**

    DROP TABLE table_name

**重命名表名**

    ALTER TABLE old_table_name RENAME new_table_name

**向表中添加一列**

    ALTER TABLE table_name ADD column_name 数据类型

**删除表中的一列**

    ALTER TABLE table_name DROP COLUMN column_name

**修改一列的数据类型**

    ALTER TABLE table_name MODIFY column_name 数据类型

**重命名一个列**

    ALTER TABLE table_name CHANGE COLUMN old_column_name new_column_name 数据类型

**插入数据**

    INSERT INTO table_name VALUES (值1,值2,...)

    INSERT INTO table_name ( 列1,列2)  VALUES ( 值1,值2)

**查询数据**

    SELECT * FROM table_name

    SELECT column1,column2 FROM table_name

**条件查询**

    SELECT * FROM table_name WHERE 条件

    =        等于

    <>       不等于

    >        大于

    <        小于

    >=       大于等于

    <=       小于等于

    BETWEEN  在某个范围之间

    LIKE     某种搜索模式

    AND      与

    OR       或

    BETWEEN

    SELECT * FROM user WHERE id BETWEEN 5 AND 7;

    查询id为5、6，7的user，userId范围是包含边界值的

    NOT  BETWEEN 的范围是不包含边界值

    LIKE

    % 匹配任意类型和长度的字符

    _ 匹配单个任意字符

    SELECT * FROM user WHERE u_name LIKE ‘%三_'

**删除操作**

    DELETE FROM table_name WHERE 条件

    DELETE * FROM table_name

**更新数据**

    UPDATE table_name SET column = new_value WHERE 条件


**清除重复项**

    SELECT DISTINCT * FROM table_name


**排序**

    SELECT * FORM table_name ORDER BY column_name       默认升序

    SELECT * FROM table_name ORDER BY column_name DESC  降序


#### 用户管理

**创建用户**

    CREATE USER user_name IDENTIFIED BY password   创建后需要给权限

**删除用户**

    DROP USER user_name

**重命名用户**

    RENAME USER old_user_name TO new_user_name

**修改当前密码**

    SET PASSWORD = PASSWORD(new_password)

**修改用户密码**

    SET PASSWORD FOR user_name = PASSWORD(new_password)

#### 用户权限管理

    *.*                          全局层级

    database_name.*              数据库层级

    database_name.table_name     表层级

                                 列层级

                                 子程序层级


**授予用户权限**

    GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY password    root用户默认只能本地连接,这里可以远程连接.


    %                                 所有远程主机

    www.example.com/192.168.1.1       精确主机或IP地址

    *.example.com                     通配符'*'

    192.168.1.0/255.255.255.0         指定一个网段


**删除用户权限**

    REVOKE ALL PRIVILEGES FROM user_name


#### 数据备份

    mysqldump -u root -p password database_name > backup.sql  备份

    mysqldump -u root -p password database_name < backup.sql  恢复



#### 编码

**查看mysql支持编码**

    SHOW CHARACTER SET

    SHOW VARIABLES LIKE 'character_set%'

    SHOW VARIABLES LIKE 'collation%'

**设置数据库编码**

    CREATE DATABASE database_name DEFAULT CHARACTER SET utf8 DEFAULT COLLATE uft8_general_ci


**修改数据库编码**

    ALTER DATABASE database_name CHARACTER SET utf8 COLLATE utf8_general_ci


**sql注入**

    表结构 id name      password
    表数据 1  test_name test_password

    select * from where name = 'test_name' and password = 'test_password'

    注入

    'test_name' 替换为 'or 1 = 1 #'

    用 or 1 = 1 保持查询永真 
    用# 注释后面的内容


**连表查询**

    读取共有的数据
    ELECT a.id, a.name, b.id FROM user a INNER JOIN table1 b ON a.id = b.user_id;

    读取a的所有数据
    ELECT a.id, a.name, b.id FROM user a LEFT JOIN table1 b ON a.id = b.user_id;

    读取b的所有数据
    ELECT a.id, a.name, b.id FROM user a RIGHT JOIN table1 b ON a.id = b.user_id;
