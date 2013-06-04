---
layout : post
category : lessons
tags : [��ʼ]
---

����ǳ��ѧϰmysql-����ƪ

    SQL������֮DDLƪ
        DDL(Data Definition Languages���ݶ������)
            �������ݿ�
                �﷨��CREATE DATABASE dbname��
                ���ӣ�mysql> create database test1;
            ɾ�����ݿ�
                �﷨��drop database dbname;
                ���ӣ�mysql> drop database test1;
                ע�⣺���ݿ�ɾ������������б����ݶ���ȫ��ɾ��������ɾ��ǰһ��Ҫ��ϸ��鲢������Ӧ����.
            ������
                �﷨��CREATE TABLE  tablename (column_name_1 column_type_1 constraints��column_name_2 column_type_2 constraints������column_name_n column_type_n constraints�� 
                ���ӣ�mysql> create table emp(ename varchar(10),hiredate date,sal decimal(10,2),deptno int(2));
            ɾ����
                �﷨��DROP TABLE tablename
                ���ӣ�mysql> drop table emp;
            �޸ı�
                �޸ı�����
                    �﷨��ALTER TABLE tablename MODIFY [COLUMN] column_definition [FIRST | AFTER col_name]
                    ���ӣ�mysql> alter table emp modify ename varchar(20);
                ���ӱ��ֶ�
                    �﷨��ALTER TABLE tablename ADD [COLUMN] column_definition [FIRST | AFTER col_name]
                    ���ӣ�mysql> alter table emp add column age int(3);
                ɾ�����ֶ�
                    �﷨��ALTER TABLE tablename DROP [COLUMN] col_name;
                    ���ӣ�mysql> alter table emp drop column age;
                �ֶ��޸���������
                    �﷨��ALTER TABLE tablename CHANGE [COLUMN] old_col_name_column_definition [FIRST | AFTER col_name]
                    ���ӣ�mysql> alter table emp change column age age1 int(4);
                    ע�⣺change��modify�������޸ı�Ķ��壬��ͬ����change������Ҫд���������������㡣����change���ŵ��ǿ����޸������ƣ�modify���ܡ�
                �޸��ֶ�����˳��
                    ����1��mysql> alter table emp add birth date after ename;
                    ����2��mysql> alter table emp modify age int(3) first;
                    ע��1��ǰ����ܵĵ��ֶ����Ӻ��޸��﷨��ADD/CNAHGE/MODIFY���У�����һ����ѡ��first|after column_name�����ѡ����������޸��ֶ��ڱ��е�λ�ã�Ĭ��ADD���ӵ����ֶ��Ǽ��ڱ�����λ�ã���CHANGE/MODIFYĬ�϶�����ı��ֶε�λ�á�
                    ע��2��CHANGE/FIRST|AFTER COLUMN��Щ�ؼ��ֶ�����MySQL�ڱ�׼SQL�ϵ���չ�����������ݿ��ϲ�һ�����á�
                �޸ı���
                    �﷨��ALTER TABLE tablename RENAME [TO] new_table_name
                    ���ӣ�mysql> alter table emp rename emp1;
            ������������
                �鿴���ݿ⣺mysql> show databases;
                ѡ�����ݿ⣺mysql> use test1;
                �鿴���ݿ�����ı�mysql> show tables;
                �鿴һ�±�Ķ��壺mysql> desc emp;
                �鿴�������SQL��䣺mysql> show create table emp \G;
