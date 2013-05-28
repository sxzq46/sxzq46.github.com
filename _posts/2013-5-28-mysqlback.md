---
layout : post
category : lessons
tags : [开始]
---
Mysql主从复制

MySQL支持单向、异步复制，复制过程中一个服务器充当主服务器，而一个或多个其它服务器充当从服务器。主服务器将更新写入二进制日志文件，并维护日志文件的一个索引以跟踪日志循环。当一个从服务器连接到主服务器时，它通知主服务器从服务器在日志中读取的最后一次成功更新的位置。从服务器接收从那时起发生的任何更新，然后封锁并等待主服务器通知下一次更新。

复制目的：
1．	数据分布
2．	负载均衡
3．	备份
4．	高可用和容错


第一步：安装mysql数据库(略)

第二步：配置主从服务器

一、主服务器配置
1.	启动mysql服务
/usr/local/mysql/bin/mysql start

2.	通过命令行登录管理MySQL服务器
/usr/local/mysql/bin/mysql –u root –p

3.	创建用户并授权slave
每个slave使用标准的MySQL用户名和密码连接master。进行复制操作的用户会授予REPLICATION SLAVE权限。用户名的密码都会存储在文本文件master.info中。假如，你想创建repl用户，命令如下：
mysql> GRANT REPLICATION SLAVE, REPLICATION CLIENT ON *.*
-> TO repl@192.168.56.2 IDENTIFIED BY 'p4ssword'; 
一般不用root帐号，如果@之后使用%，则表示所有客户端都可能连，只要帐号，密码正确，此处可用具体客户端IP代替，如192.168.156.2加强安全。

4.	修改master配置文件
vim /etc/my.cnf
[mysqld]
log-bin=mysql-bin   //[必须]启用二进制日志
server-id=1         //[必须]服务器唯一ID，默认是1，一般取IP最后一段
重启master mysql服务
/usr/local/mysql/bin/mysql restart

5.	查看master状态
	mysql>show master status;
	+------------------+----------+--------------+------------------+
	| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB |
	+------------------+----------+--------------+------------------+
	| mysql-bin.000002 |      106 |              |                  |
	+------------------+----------+--------------+------------------+
1	row in set (0.00 sec)




二、从服务器配置
1.	修改salve配置文件
vim /etc/my.cnf
log-bin=mysql-bin
server-id=3
relay-log=mysql-relay-bin
log-slave-updates=1
read-only=1
保存退出，重启mysql服务

2.	配置&启动slave同步
mysql> CHANGE MASTER TO MASTER_HOST='192.168.56.10',
    -> MASTER_USER='repl',
    -> MASTER_PASSWORD='p4ssword',
    -> MASTER_LOG_FILE='mysql-bin.000002',
-> MASTER_LOG_POS=106;

			mysql> SHOW SLAVE STATUS\G
			*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 192.168.56.10
                  Master_User: repl
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.000002
          Read_Master_Log_Pos: 106
               Relay_Log_File: mysql-relay-bin.000017
                Relay_Log_Pos: 251
        Relay_Master_Log_File: mysql-bin.000002
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB: 
          Replicate_Ignore_DB: 
           Replicate_Do_Table: 
       Replicate_Ignore_Table: 
      Replicate_Wild_Do_Table: 
  Replicate_Wild_Ignore_Table: 
                   Last_Errno: 0
                   Last_Error: 
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 106
              Relay_Log_Space: 551
              Until_Condition: None
               Until_Log_File: 
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File: 
           Master_SSL_CA_Path: 
              Master_SSL_Cert: 
            Master_SSL_Cipher: 
               Master_SSL_Key: 
        Seconds_Behind_Master: 0
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error: 
               Last_SQL_Errno: 0
               Last_SQL_Error: 
1 row in set (0.00 sec)
		
注：Slave_IO及Slave_SQL进程必须正常运行，即YES状态，否则都是错误的状态(如：其中一个NO均属错误)。

以上操作过程，主从服务器配置完成。

第三步：主从服务器测试

一、主服务器建立数据库：
mysql> create database shixz;
Query OK, 1 row affected (0.07 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| shixz              |
| test               |
+--------------------+
4 rows in set (0.03 sec)







二、从服务器查询数据库：
	mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| shixz              |
| test               |
+--------------------+
4 rows in set (0.07 sec)
至此，已经成功将master上数据复制到slave上。

总结：本次实验是最基本的mysql主从配置。在真实环境中会面临更多的问题，比如在做主从之前，master已有数据那就必须导出数据，做完主从之后再导入。诸如此类问题，在后面我会将网上的一些案例录入进来。之后将深入研究mysql的读写分离及深入复制。

