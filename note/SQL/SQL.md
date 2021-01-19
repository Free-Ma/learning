- SQL是一种数据库查询和设计语言
- 使得数据库可以通过命令行的方式而非图形化界面的方式对表进行增删改查等操作
- SQL是一门独立的语言，在开发语言中可以嵌入SQL对数据库进行操作

1. DDL语句 数据库定义语言

   create alter drop

2. DML语言 数据库操作语言对表中数据的操作

   insert 插入 delete删除 update改.......

   - insert

     ```sql
     insert into test(id, name, gender) values('1','张三','男');
     insert into test values(value1,value2.....);-- 必须与字段一一对应
     insert into test values(value1,value2....),(value1,value2...);-- 增加多条数据
     ```
   - DELETE删除表数据
     
      - 和drop的区别：delete只是把表中的数据给删除了，表还在留着；drop是把表和数据一并给删除了
   ```sql
   delete from test;-- 清空test表的数据
   delete from test where gender='男';-- 删除指定条件的数据
   delete from test where gender='男' and age>'25';-- 删除同时满足多个条件的数据
   delete from test where gender='男' or gender='女' and age>'25';
   ```
   - UPDATE 修改表数据
     ```sql
     UPDATE test SET salary='28000';-- 修改单条数据
     UPDATE test SET salary='35000',work_time='160',......;-- 修改多条数据
     UPDATE test SET gender='男' WHERE name='张三';-- 修改指定条件的数据
     ```
   - SELECT 查询
     ```sql
     SELECT * FROM test;-- 查询test表中的所有信息，*表示所有数据
     SELECT name from student;
     SELECT name,age,gender FROM student;
     SELECT DISTINCT native_place FROM student;-- 去重复
     SELECT 姓名 'name',年龄 'age' FROM student;-- 别名1
     SELECT 姓名 as 'name',年龄 as 'age' FROM student;-- 别名2
     SELECT no,course,score,score+10 as '新成绩' FROM student;
     SELECT no,course,score FROM student where native_place='湖北';-- 条件查询
     SELECT No,course,score FROM student where score BETWEEN 60 AND 100;-- 搜索在范围内的所有数据
     SELECT No,course,score FROM student where score NOT BETWEEN 30 AND 59;-- 搜索不在范围内的所有数据
     SELECT No,name,gender,province FROM student WHERE province IN('hubei','nanjing');-- 搜索匹配值，只要满足一个就会有查询结果
     SELECT No,name,gender,province FROM student WHERE province NOT IN('hubei','nanjing');-- 搜索匹配值之外的数据
     SELECT No,name,privince FROM where name like '李%';-- 模糊查询，%代表零或多个字符，_代表一个字符，NOT LIKE模糊查询不包括此条件的数据
     SELECT * FROM province IS NOT NULL;-- 查询条件不为空的数据，IS NULL查询条件为空的数据
     SELECT No,name,province FROM test LIMIT 2，5;-- 从第2行开始，可选，不写的话从0开始;总共要查询5行
     SELECT 部门编号,sum(员工编号) FROM emp_info GOUP BY 部门编号;-- 查询每个部门的人数，GOUP BY以部门编号分组
     SELECT 部门编号,sum(员工编号) FROM emp_info GOUP BY 部门编号 HAVING AVG(salary)>18000;-- having 必须与group by一起使用，having一般情况下都是聚合函数当作条件，但是where不能写聚合函数
     SELECT * FROM test order by score;-- 默认情况下，是升序asc排列，降序为desc
     ```
      - 聚合函数  `SELECT 聚合函数 FROM 表名;`
      ```sql
      SUM([DISTINCT] 列名)-- 对某个列进行求和
      AVG([DISTINCT] 列名)-- 求某个列的平均值
      MAX([DISTINCT] 列名)-- 求某个列的最大值
      MIN([DISTINCT] 列名)-- 求某个列的最小值
      COUNT(*)-- 统计表中元组个数
      COUNT([DISTINCT] 列名)-- 统计此列的个数
      ```
      [DISTINCT]去重可选
      除了COUNT(*)外，其他函数操作时，均忽略空值(null)