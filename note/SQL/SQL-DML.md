- SQL是一种数据库查询和设计语言
- 使得数据库可以通过命令行的方式而非图形化界面的方式对表进行增删改查等操作
- SQL是一门独立的语言，在开发语言中可以嵌入SQL对数据库进行操作

1. DDL语句 数据库定义语言

   create alter drop

2. DML语言 数据库操作语言对表中数据的操作

  INSERT 插入 DELETE删除 UPDATE改.......

  - INSERT

    ```sql
    INSERT INTO test(id, name, gender) VALUES('1','张三','男');
    INSERT INTO test VALUES(value1,value2.....);-- 必须与字段一一对应
    INSERT INTO test VALUES(value1,value2....),(value1,value2...);-- 增加多条数据
    ```
  - DELETE删除表数据
     
    - 和drop的区别：DELETE只是把表中的数据给删除了，表还在留着；drop是把表和数据一并给删除了
      ```sql
      DELETE FROM test;-- 清空test表的数据
      DELETE FROM test WHERE gender='男';-- 删除指定条件的数据
      DELETE FROM test WHERE gender='男' AND age>'25';-- 删除同时满足多个条件的数据
      DELETE FROM test WHERE gender='男' OR gender='女' AND age>'25';
      ```
  - UPDATE 修改表数据
    ```sql
    UPDATE test SET salary='28000';-- 修改单条数据
    UPDATE test SET salary='35000',wORk_time='160',......;-- 修改多条数据
    UPDATE test SET gender='男' WHERE name='张三';-- 修改指定条件的数据
    ```
     
  - SELECT 查询
    ```sql
    SELECT * FROM test;-- 查询test表中的所有信息，*表示所有数据
    SELECT name FROM student;
    SELECT name,age,gender FROM student;
    SELECT DISTINCT native_place FROM student;-- 去重复
    SELECT 姓名 'name',年龄 'age' FROM student;-- 别名1
    SELECT 姓名 AS 'name',年龄 AS 'age' FROM student;-- 别名2
    SELECT no,course,scORe,scORe+10 AS '新成绩' FROM student;
    SELECT no,course,scORe FROM student WHERE native_place='湖北';-- 条件查询
    SELECT No,course,scORe FROM student WHERE scORe BETWEEN 60 AND 100;-- 搜索在范围内的所有数据
    SELECT No,course,scORe FROM student WHERE scORe NOT BETWEEN 30 AND 59;-- 搜索不在范围内的所有数据
    SELECT No,name,gender,province FROM student WHERE province IN('hubei','nanjing');-- 搜索匹配值，只要满足一个就会有查询结果
    SELECT No,name,gender,province FROM student WHERE province NOT IN('hubei','nanjing');-- 搜索匹配值之外的数据
    SELECT No,name,privince FROM WHERE name like '李%';-- 模糊查询，%代表零或多个字符，_代表一个字符，NOT LIKE模糊查询不包括此条件的数据
    SELECT * FROM province IS NOT NULL;-- 查询条件不为空的数据，IS NULL查询条件为空的数据
    SELECT No,name,province FROM test LIMIT 2，5;-- 从第2行开始，可选，不写的话从0开始;总共要查询5行
    SELECT 部门编号,sum(员工编号) FROM emp_info GOUP BY 部门编号;-- 查询每个部门的人数，GOUP BY以部门编号分组
    SELECT 部门编号,sum(员工编号) FROM emp_info GOUP BY 部门编号 HAVING AVG(salary)>18000;-- having 必须与GROUP BY一起使用，having一般情况下都是聚合函数当作条件，但是WHERE不能写聚合函数
    SELECT * FROM test ORDER BY scORe;-- 默认情况下，是升序ASc排列，降序为DESC
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
        
      除了COUNT(*)外，其他函数操作时，均忽略空值(NULL)
     
  - MYSQL函数
    - 字符串函数:
      - LENGTH(name):求出name的字节长度
      - CHAR_LENGTH(name):求出name的字符长度
        - 一个汉字占用3个字节空间 
      - MID(name,2,1): 截取指定字符串，name代表要截取的字段对应的数据，2代表开始位置(最小值是1)，1代表要截取的长度
    - 数值函数 
      - ROUND的基本作用是四舍五入
        - ROUND(原始数据,保留的小数位)
        - 不设置小数位，默认保留整数位
      - LEAST(1,2,3,4,5,6)查找最小的数字，GREATEST查找最大数字，必须要放具体数字，无法根据字段名进行工作
    - 日期时间函数
      - `SELECT NOW();` 当前日期和时间
      - `SELECT CURRENT_DATE();` 当前日期
      - `SELECT CURRENT_TIME();` 当前时间
      - `SELECT TO_DAYS('2020-01-01');` 已经过了多少天
      - `SELECT DAYOFYEAR(NOW());` 求得该年已过多少天
      - `SELECT WEEK(NOW());` 当前的时日是第几周
    - 控制函数
      - IF有三个参数，第一个参数为空输出第三个参数，否则输出第二个
        - `SELECT IF(布尔表达式,参数1,参数2);`
        - `SELECT IF(NULL,参数1,参数2);`
      - IFNULL有两个参数，第一个参数为NULL输出第二个参数，否则输出第一个
        - `SELECT IFNULL(NULL,参数2);`
        - `SELECT IFNULL(参数1,参数2);`

  - 表连接