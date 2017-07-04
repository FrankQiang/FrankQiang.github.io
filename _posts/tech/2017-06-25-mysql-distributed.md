---
layout: post
title: Mysql ： master/slave
category: 技术
tags: mysql 分布式
keywords: mysql 分布式
---

### Master

#### 配置文件


```CPP
					    # master.cnf
[mysqld]

server-id = 1           # 必须唯一 
log-bin = mysql-bin      

binlog-do-db=moments    # 需要同步的数据库
binlog-ignore-db=mysql  # 忽略同步的数据库
```

#### 创建slave用户并获得复制权限

```CPP
CREATE USER 'slave'@'%' IDENTIFIED BY 'slave_password';
GRANT REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'slave'@'%';
```

#### 获取master 状态

如果master 有数据操作，需要锁定数据库
flush tables with read lock

完成主从复制后解锁
unlock tables;

获取master 状态
```CPP
mysql> show master status;
+------------------+-----------+--------------+------------------+-------------------+
| File             | Position  | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
+------------------+-----------+--------------+------------------+-------------------+
| mysql-bin.000004 | 23719383  | moments      | mysql            |                   |
+------------------+-----------+--------------+------------------+-------------------+
1 row in set (0.00 sec)
```

### Slave

#### 配置文件


```CPP
                        # slave.cnf
[mysqld]

server-id = 2 
log-bin = mysql-bin 
read_only  = 1          # 打开只读选项
```

#### 配置复制

```CPP
change master to master_host='172.17.0.2',             # master ip 
                 master_user='slave',                  # 在master创建的用户 
                 master_password='slave_password',     # 在master创建的用户密码 
                 master_port=3306,                     # master port
                 master_log_file='mysql-bin.000004',   # 从 master status 获取 
                 master_log_pos=23719383,              # 从 master status 获取 
                 master_connect_retry=30;
```

#### 检查是否正常复制

```CPP
start slave;       # 启动

show slave status;

Slave_IO_Running 和 Slave_SQL_Running 均为Yes，则表示连接正常。

```
