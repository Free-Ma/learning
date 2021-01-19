　　MySQL数据库是一种关系型数据库管理系统，是一种开源软件由瑞典MySQL AB公司开发，08年1月16日被Sun公司收购，09年Sun公司又被Oracle公司收购

### ＭySQL优点：

- 体积小、速度快、成本低、开源，可以和开发语言来结合，可以移植性（跨平台），可以在不同的操作系统中使用

**查看数据库：`show databases;`**

**创建数据库：`create database test`**

**删除数据库：`drop database test`**

**数据类型:**

1. 数值型
   - 整数类型:`int`
   - 小数类型:`decimal`
   - 小数格式:`decimal(总长度,小数位)`,例如`decimal(5,2)`:`123.45`
2. 日期和时间类型
   - datetime:`YYYY-MM-DD HH:MM:SS`
   - date: `YYYY-MM-DD`
3. 字符串类型
   - char(字符串的长度)  定长
   - varchar(字符串的长度)  变长

**数据库注释：**

- 单行：-- this is a comment
- 多行：/*multiline comment\*/

**切换数据库**

- 格式：use test;

**创建表**

```sql
create table student(
	学号 varchar(10),
    姓名 varchar(15),
    年龄 int,
    性别 char(2)
);
```

**打开表：查看表数据**

**设计表：查看设计表结构**

**删除表**：

- 单表  `drop table 表名;`
- 多表  `drop table 表名1,表名2,表名3;`

**添加列**

- `alter table 表名 add 属性名 数据类型`

**删除列**

- `alter table 表名 drop 属性名`

**修改属性**

- `alter table 表名 modify 列名 新的数据类型;`

**修改字段名**

- `alter table 表名 change 旧字段名 新字段名 数据类型;`

**显示表结构**

`desc 表名`

主键约束:唯一,不重复,不能为空 primary key

1. 创建表的同时创建主键约束

   - 格式:

     ```sql
     create table test(-- 通用
     	列名 数据类型 primary key;
         列名2 数据类型
     );
     create table 表名(
     	列名1 数据类型,
         列名2 数据类型,
         constraint 主键约束的名字 primary key(列名1)
     );
     create table 表名(
     	列名1 数据类型,
         列名2 数据类型,
         primary key(列名1)
     );
     ```
   
2. 针对已经存在的表,添加和删除主键约束
   - 添加
      ```sql
      alter table test modify user_id int primary key;
      alter table test add primary key(user_id);
      alter table test add constraint PK_id primary key(user_id);-- 通用
      ```
      
   - 删除
      `alter table test drop primary key;`
   
3. 联合主键：指的是把两个列看成一个整体，这个整体是不为空，唯一，不重复

   - 格式
     ```sql
     create table test(
        emp_no int,
        dept_no int,
        constraint PK_no primary key(emp_no,dept_no)
     );
     create table test(
        emp_no int,
        dept_no int,
        primary key(emp_no,dept_no)
     );
     alter table test add primary key(emp_no,dept_no);
     alter table test constraint PK_no primary key(emp_no,dept_no);
     ```
4. 唯一约束：
   - 指定table的列或列组合不能重复，保证数据的唯一性；
   - 可以为多个null
   - 同一个表可以有多个唯一约束，多个列组合的约束
   - 如果不给唯一约束名称，就默认和列名相同
   - mysql会给唯一约束的列上默认创建一个唯一索引
   - 格式
      ```sql
      create table test(
         id int,
         emp_id int,
         constraint PK_id unique(id),
         constraint PK_id unique(emp_id)
      );-- 通用
      create table test(
         id int unique,
         emp_id int unique,
         dept_id
      );
      alter table test add unique(id,emp_id);-- 已有表添加唯一约束
      alter table test drop index id;-- 删除约束
      ```
5. **域完整性约束：保证表中不会输入无效的数据**
   - 默认约束  default 当默认约束来修饰某个列的时候，修饰的列即使不写数据也会默认一个值
      ```sql
      create table test(
         id int default '10',
         gender char(2) default 'man'
      );-- 创建表时添加
      alter table test modify id int(10) default 2;-- 已有表添加
      alter table test modify id int;-- 删除约束
      ```
   - 非空约束  not null 当前列必须有值
       ```sql
       create tale test(
          id int not null,
          name varchar(10) not null,
          salary double(6,2)
       );-- 建表时添加
       alter table test modify id int not null;-- 已有表添加
       ```
6. 参照完整性
   - 指表与表之间的数据参照引用
   - 使用外键约束实现
   - 外键约束 foreign key
      - 构建于一个表的两个字段或是两个表的两个字段之间的参照关系
      - 表的外键值必须在主表中能找到
      ```sql
      create table test(
         id int primary key,
         name varchar(10)
      );
      create table test2(
         id int,
         name2 varchar(10),
         constraint id foreign key(id) references test(id)
      );-- 创建表时添加外键约束
      
      alter table test2 add constraint FK_id foreign key(id) references test(id);-- 已有表添加外键约束
      alter table test2 drop foreign key FK_id;
      ```