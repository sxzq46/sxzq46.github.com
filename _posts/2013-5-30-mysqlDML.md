---
layout : post
category : lessons
tags : [开始]
---

mysql基础篇

    SQL语句分类
    DML(Data Manipulation Language数据操纵语句)
        DML简介
            DML操作是指对数据库中表记录的操作，主要包括表记录的插入（insert）、更新（update）、删除（delete）和查询（select），是开发人员日常使用最频繁的操作。下面将依次对它们进行介绍。
        插入记录
            语法：INSERT INTO  tablename (field1,field2,……fieldn) VALUES(value1,value2,……valuesn);
            例子1：mysql> insert into emp (ename,hiredate,sal,deptno) values('zzx1','2000-01-01','2000',1);
            例子2：也可以不用指定字段名称，但是values后面的顺序应该和字段的排列顺序一致： 
mysql> insert into emp  values('lisa','2003-02-01','3000',2); 
            例子3：对于含可空字段、非空但是含有默认值的字段、自增字段，可以不用在insert后的字段列表里面出现，values后面只写对应字段名称的value，这些没写的字段可以自动设置为NULL、默认值、自增的下一个数字，这样在某些情况下可以大大缩短SQL语句的复杂性。 
mysql> insert into emp  (ename,sal) values('dony',1000);
            【多条记录插入】
语法:INSERT INTO  tablename  (field1, field2,……fieldn)
VALUES
(record1_value1, record1_value2,……record1_valuesn),
(record2_value1, record2_value2,……record2_valuesn),
……
(recordn_value1, recordn_value2,……recordn_valuesn);
例子：mysql> insert into dept values(5,'dept5'),(6,'dept6');
        更新记录
            语法：UPDATE tablename SET field1=value1，field2.=value2，……fieldn=valuen [WHERE CONDITION]
            例子：mysql> update emp set sal=4000 where ename='lisa';
            【多表数据更新】
语法：UPDATE t1,t2…tn set t1.field1=expr1,tn.fieldn=exprn  [WHERE CONDITION]
例子：mysql> update emp a,dept b set a.sal=a.sal*b.deptno,b.deptname=a.ename where a.deptno=b.deptno;
            注意：多表更新的语法更多地用在了根据一个表的字段，来动态的更新另外一个表的字段
        删除记录
            语法：DELETE FROM tablename [WHERE CONDITION]
            例子：mysql> delete from emp where ename='dony';
            【多表数据删除】
语法：DELETE t1,t2…tn FROM t1,t2…tn [WHERE CONDITION]
例子：mysql> delete a,b from emp a,dept b where a.deptno=b.deptno and a.deptno=3;
注意：如果from后面的表名用别名，则delete后面的也要用相应的别名，否则会提示语法错误。
            注意：注意：不管是单表还是多表，不加where条件将会把表的所有记录删除，所以操作时一定要小心。
        查询记录
            基础查询
                语法：SELECT * FROM tablename [WHERE CONDITION]
                例子1：mysql> select * from emp;
                例子2：mysql> select ename,hiredate,sal,deptno from emp;（单个字段单个数据查询 ）
                注意：*用来查询所有字段信息，逗号用来分割查询字段
            不重复记录查询
                使用distinct关键字将表中的记录去掉重复后显示出来
                例子：mysql> select distinct deptno from emp;
            条件查询
                使用where关键字根据限定条件来查询一部分数据
                例子：mysql> select * from emp where deptno=1;
                多条件联合查询
例子：mysql> select * from emp where deptno=1 and sal<3000;
注意：where后面的条件除了‘=’外，还可以使用>、<、>=、<=、!=等比较运算符；多个条件之间还可以使用or、and等逻辑运算符进行多条件联合查询。
            排序和限制
                排序
                    使用ORDER BY关键字取出按照某个字段进行排序后的记录结果集
                    语法：SELECT * FROM tablename [WHERE CONDITION] [ORDER BY field1 [DESC|ASC]，field2 [DESC|ASC]，……fieldn [DESC|ASC]]；
                    例子1：mysql> select * from emp order by sal;
                    例子2：mysql> select * from emp order by deptno,sal desc;
                    注意：
1.DESC表示按照字段进行降序排列，ASC则表示升序排列，如果不写此关键字默认是升序排列。
2.如果排序字段的值一样，则值相同的字段按照第二个排序字段进行排序，以此类推。如果只有一个排序字段，则这些字段相同的记录将会无序排列。
                限制
                    使用limit关键字只显示一部分记录
                    语法：SELECT ……[LIMIT offset_start,row_count]
                    例子1：mysql> select * from emp order by sal limit 3;
                    例子2：显示emp表中按照sal排序后从第二条记录开始，显示3条记录
mysql> select * from emp order by sal limit 1,3;
                    注意：
1.limit经常和order by一起配合使用来进行记录的分页显示。
2.limit属于MySQL扩展SQL92后的语法，在其他数据库上并不能通用。
            聚合
                语法： SELECT [field1,field2,……fieldn] fun_name  FROM tablename [WHERE where_contition] [GROUP BY field1,field2,……fieldn [WITH ROLLUP]] [HAVING where_contition] 
                参数：
fun_name表示要做的聚合操作，也就是聚合函数，常用的有sum（求和）、count(*)（记录数）、max（最大值）、min（最小值）。
GROUP BY关键字表示要进行分类聚合的字段，比如要按照部门分类统计员工数量，部门就应该写在group by后面。
WITH ROLLUP是可选语法，表明是否对分类聚合后的结果进行再汇总。
HAVING关键字表示对分类后的结果再进行条件的过滤。

                例子：
1.mysql> select count(1) from emp;统计总人数
2.mysql> select deptno,count(1) from emp group by deptno;在上基础上统计各个部门的人数
3.mysql> select deptno,count(1) from emp group by deptno with rollup;既统计各部门人数，又统计总人数
4.mysql> select deptno,count(1) from emp group by deptno having count(1)>1;统计人数大于1人的部门
5.mysql> select sum(sal),max(sal),min(sal) from emp;统计所有员工的薪水总额、最高和最低薪水
            表连接
                同时显示多个表中的字段
                表连接分为内连接和外连接，它们的最主要区别是內连接仅选出两张表中互相匹配的记录，而外连接会选出其他不匹配的记录。
                内连接
                    例子：mysql> select ename,deptname from emp,dept where emp.deptno=dept.deptno; 
                外连接
                    外连接有分为左连接和右连接
左连接：包含所有的左边表中的记录甚至是右边表中没有和它匹配的记录
右连接：包含所有的右边表中的记录甚至是左边表中没有和它匹配的记录
                    左连接例子：mysql> select ename,deptname from emp left join dept  on  emp.deptno=dept.deptno;
                    右连接例子：mysql> select ename,deptname from dept right join emp on dept.deptno=emp.deptno;
            子查询
                当我们查询的时候，需要的条件是另外一个select语句的结果，这个时候，就要用到子查询。
                例子：mysql> select * from emp where deptno in(select deptno from dept);
                mysql> select * from emp where deptno in(select deptno from dept);
转换为表连接
mysql> select emp.* from emp ,dept where emp.deptno=dept.deptno;
                注意：子查询和表连接之间的转换主要应用在两个方面：
1.MySQL 4.1以前的版本不支持子查询，需要用表连接来实现子查询的功能
2.表连接在很多情况下用于优化子查询

            记录联合
                用union和union all关键字将两个表的数据按照一定的查询条件查询出来后，将结果合并到一起显示出来
                语法：SELECT * FROM t1 UNION|UNION ALL SELECT * FROM t2 
                例子：mysql> select deptno from emp union|union all select deptno from dept;

				