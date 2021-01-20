　　MySQL数据库是一种关系型数据库管理系统，是一种开源软件由瑞典MySQL AB公司开发，08年1月16日被Sun公司收购，09年Sun公司又被Oracle公司收购

### MySQL优点：

- 体积小、速度快、成本低、开源，可以和开发语言来结合，可以移植性（跨平台），可以在不同的操作系统中使用

**查看数据库：`SHOW DATABASES;`**

**创建数据库：`CREATE DATABASE test`**

**删除数据库：`DROP DATABASE test`**

**数据类型:**

1. 数值型
   - 整数类型:`INT`
   - 小数类型:`decimal`
   - 小数格式:`decimal(总长度,小数位)`,例如`decimal(5,2)`:`123.45`
2. 日期和时间类型
   - datetime:`YYYY-MM-DD HH:MM:SS`
   - date: `YYYY-MM-DD`
3. 字符串类型
   - CHAR(字符串的长度)  定长
   - VARCHAR(字符串的长度)  变长

**数据库注释：**

- 单行：-- this is a comment
- 多行：/*multiline comment\*/

**切换数据库**

- 格式：use test;

**创建表**

```sql
CREATE TABLE student(
	学号 VARCHAR(10),
    姓名 VARCHAR(15),
    年龄 INT,
    性别 CHAR(2)
);
```

**打开表：查看表数据**

**设计表：查看设计表结构**

**删除表**：

- 单表  `DROP TABLE 表名;`
- 多表  `DROP TABLE 表名1,表名2,表名3;`

**添加列**

- `ALTER TABLE 表名 ADD 属性名 数据类型`

**删除列**

- `ALTER TABLE 表名 DROP 属性名`

**修改属性**

- `ALTER TABLE 表名 MODIFY 列名 新的数据类型;`

**修改字段名**

- `ALTER TABLE 表名 CHANGE 旧字段名 新字段名 数据类型;`

**显示表结构**

`desc 表名`

主键约束:唯一,不重复,不能为空 PRIMARY KEY

1. **创建表的同时创建主键约束**

   - 格式:

     ```sql
     CREATE TABLE test(-- 通用
     	列名 数据类型 PRIMARY KEY;
         列名2 数据类型
     );
     CREATE TABLE 表名(
     	列名1 数据类型,
         列名2 数据类型,
         CONSTRAINT 主键约束的名字 PRIMARY KEY(列名1)
     );
     CREATE TABLE 表名(
     	列名1 数据类型,
         列名2 数据类型,
         PRIMARY KEY(列名1)
     );
     ```
   
2. **针对已经存在的表,添加和删除主键约束**
   - 添加
      ```sql
      ALTER TABLE test MODIFY user_id INT PRIMARY KEY;
      ALTER TABLE test ADD PRIMARY KEY(user_id);
      ALTER TABLE test ADD CONSTRAINT PK_id PRIMARY KEY(user_id);-- 通用
      ```
      
   - 删除
      `ALTER TABLE test DROP PRIMARY KEY;`
   
3. **联合主键：指的是把两个列看成一个整体，这个整体是不为空，唯一，不重复**

   - 格式
     ```sql
     CREATE TABLE test(
        emp_no INT,
        dept_no INT,
        CONSTRAINT PK_no PRIMARY KEY(emp_no,dept_no)
     );
     CREATE TABLE test(
        emp_no INT,
        dept_no INT,
        PRIMARY KEY(emp_no,dept_no)
     );
     ALTER TABLE test ADD PRIMARY KEY(emp_no,dept_no);
     ALTER TABLE test CONSTRAINT PK_no PRIMARY KEY(emp_no,dept_no);
     ```
4. **唯一约束：**
   - 指定TABLE的列或列组合不能重复，保证数据的唯一性；
   - 可以为多个null
   - 同一个表可以有多个唯一约束，多个列组合的约束
   - 如果不给唯一约束名称，就默认和列名相同
   - mysql会给唯一约束的列上默认创建一个唯一索引
   - 格式
      ```sql
      CREATE TABLE test(
         id INT,
         emp_id INT,
         CONSTRAINT PK_id UNIQUE(id),
         CONSTRAINT PK_id UNIQUE(emp_id)
      );-- 通用
      CREATE TABLE test(
         id INT UNIQUE,
         emp_id INT UNIQUE,
         dept_id
      );
      ALTER TABLE test ADD UNIQUE(id,emp_id);-- 已有表添加唯一约束
      ALTER TABLE test DROP index id;-- 删除约束
      ```
5. **域完整性约束：保证表中不会输入无效的数据**
   - 默认约束  default 当默认约束来修饰某个列的时候，修饰的列即使不写数据也会默认一个值
      ```sql
      CREATE TABLE test(
         id INT default '10',
         gender CHAR(2) default 'man'
      );-- 创建表时添加
      ALTER TABLE test MODIFY id INT(10) default 2;-- 已有表添加
      ALTER TABLE test MODIFY id INT;-- 删除约束
      ```
   - 非空约束  NOT NULL 当前列必须有值
       ```sql
       create table test(
          id INT NOT NULL,
          name VARCHAR(10) NOT NULL,
          salary DOUBLE(6,2)
       );-- 建表时添加
       ALTER TABLE test MODIFY id INT NOT NULL;-- 已有表添加
       ```
6. **参照完整性**
   - 指表与表之间的数据参照引用
   - 使用外键约束实现
   - 外键约束 FOREIGN KEY
      - 构建于一个表的两个字段或是两个表的两个字段之间的参照关系
      - 表的外键值必须在主表中能找到
      ```sql
      CREATE TABLE test(
         id INT PRIMARY KEY,
         name VARCHAR(10)
      );
      CREATE TABLE test2(
         id INT,
         name2 VARCHAR(10),
         CONSTRAINT id FOREIGN KEY(id) REFERENCES test(id)
      );-- 创建表时添加外键约束
      
      ALTER TABLE test2 ADD CONSTRAINT FK_id FOREIGN KEY(id) REFERENCES test(id);-- 已有表添加外键约束
      ALTER TABLE test2 DROP FOREIGN KEY FK_id;
      ```