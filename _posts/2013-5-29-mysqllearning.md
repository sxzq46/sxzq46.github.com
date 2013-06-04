---
layout : post
category : lessons
tags : [开始]
---

深入浅出学习mysql-基础篇

    SQL语句分类之DDL篇
        DDL(Data Definition Languages数据定义语句)
            创建数据库
                语法：CREATE DATABASE dbname；
                例子：mysql> create database test1;
            删除数据库
                语法：drop database dbname;
                例子：mysql> drop database test1;
                注意：数据库删除后，下面的所有表数据都会全部删除，所以删除前一定要仔细检查并做好相应备份.
            创建表
                语法：CREATE TABLE  tablename (column_name_1 column_type_1 constraints，column_name_2 column_type_2 constraints，……column_name_n column_type_n constraints） 
                例子：mysql> create table emp(ename varchar(10),hiredate date,sal decimal(10,2),deptno int(2));
            删除表
                语法：DROP TABLE tablename
                例子：mysql> drop table emp;
            修改表
                修改表类型
                    语法：ALTER TABLE tablename MODIFY [COLUMN] column_definition [FIRST | AFTER col_name]
                    例子：mysql> alter table emp modify ename varchar(20);
                增加表字段
                    语法：ALTER TABLE tablename ADD [COLUMN] column_definition [FIRST | AFTER col_name]
                    例子：mysql> alter table emp add column age int(3);
                删除表字段
                    语法：ALTER TABLE tablename DROP [COLUMN] col_name;
                    例子：mysql> alter table emp drop column age;
                字段修改名字类型
                    语法：ALTER TABLE tablename CHANGE [COLUMN] old_col_name_column_definition [FIRST | AFTER col_name]
                    例子：mysql> alter table emp change column age age1 int(4);
                    注意：change和modify都可以修改表的定义，不同的是change后面需要写两次列名，不方便。但是change的优点是可以修改列名称，modify则不能。
                修改字段排列顺序
                    例子1：mysql> alter table emp add birth date after ename;
                    例子2：mysql> alter table emp modify age int(3) first;
                    注意1：前面介绍的的字段增加和修改语法（ADD/CNAHGE/MODIFY）中，都有一个可选项first|after column_name，这个选项可以用来修改字段在表中的位置，默认ADD增加的新字段是加在表的最后位置，而CHANGE/MODIFY默认都不会改变字段的位置。
                    注意2：CHANGE/FIRST|AFTER COLUMN这些关键字都属于MySQL在标准SQL上的扩展，在其他数据库上不一定适用。
                修改表名
                    语法：ALTER TABLE tablename RENAME [TO] new_table_name
                    例子：mysql> alter table emp rename emp1;
            其他操作命令
                查看数据库：mysql> show databases;
                选择数据库：mysql> use test1;
                查看数据库里面的表：mysql> show tables;
                查看一下表的定义：mysql> desc emp;
                查看创建表的SQL语句：mysql> show create table emp \G;
