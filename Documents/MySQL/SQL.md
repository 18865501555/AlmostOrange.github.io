## SQL语言

- `DDL`(Data Definition Language)(数据定义语言)
  - 用来定义数据库对象:数据库、表、列等
  - `Create` `Alter` `Drop`
- `DML`(Data Manipulation Language)(数据操作语言)
  - 用来对数据库中表的记录进行更新
  - `Insert` `Update` `Delete`
- `DQL`(Data Query Language)(数据查询语言)
  - 用来查询数据库中表的记录
  - `Select`
- `DCL`(Data Control Language)(数据控制语言)
  - 用来定义数据库访问权限和安全级别，创建用户等
  - `Grant`

---

### `DDL`语句

#### 数据库相关

|          | 名称                   | **格式**                                           |
| -------- | ---------------------- | -------------------------------------------------- |
| **创建** | 创建数据库             | `create database 数据库名;`                        |
|          | 创建指定字符集的数据库 | `create database 数据库名 character set utf8/gbk;` |
|          |                        |                                                    |
| **删除** | 删除数据库             | `drop database 数据库名;`                          |
|          |                        |                                                    |
| **使用** | 使用数据库             | `use 数据库名;`                                    |
|          |                        |                                                    |
| **查询** | 查询所有数据库         | `show databases;`                                  |
|          | 查看数据库详情         | `show create database 数据库名;`                   |

#### 表相关

|          | 名称                               | 格式                                                         |
| -------- | ---------------------------------- | ------------------------------------------------------------ |
| **创建** | 创建表                             | `create table 表名(字段1名 类型,字段2名 类型);`              |
|          | 指定字符集创建表                   | `create table 表名(字段1名 类型,字段2名 类型)charset=utf8/gbk;` |
|          |                                    |                                                              |
| **增**   | 添加表字段（最后面添加）           | `alter table 表名 add 字段名 类型;`                          |
|          | 添加表字段（最前面添加）           | `alter table 表名 add 字段名 类型 first;`                    |
|          | 添加表字段（在某个字段的后面添加） | `alter table 表名 add 字段名 类型 after 字段名;`             |
|          |                                    |                                                              |
| **删**   | 删除表                             | `drop table 表名;`                                           |
|          | 删除表字段                         | `alter table 表名 drop 字段名;`                              |
|          |                                    |                                                              |
| **改**   | 修改表名                           | `rename table 原名 to 新名;`                                 |
|          | 修改表字段                         | `alter table 表名 change 原名 新名 新类型;`                  |
|          | 修改字段排列顺序                   | `alter table 表名 modify 字段名1 类型 first/after 字段名2;`  |
|          |                                    |                                                              |
| **查**   | 查询所有表                         | `show tables;`                                               |
|          | 查询表详情                         | `show create table 表名 \G;`                                 |
|          | 查询表字段                         | `desc 表名;`                                                 |



---

### `DML`语句

#### 基本语法

|        | 名称         | 格式                                                  | 备注                                           |
| ------ | ------------ | ----------------------------------------------------- | ---------------------------------------------- |
| **增** | 全表插入     | `insert into 表名 values(值1,值2);`                   | ***值的数量和顺序必须和表字段数量和顺序一致*** |
|        | 指定字段插入 | `insert into 表名(字段1名,字段2名) values(值1,值2); ` | ***值的数量和顺序必须和指定的字段一致***       |
|        | 批量插入     | `insert into 表名 values(值1,值2),(值1,值2);`         |                                                |
|        | 插入中文问题 | ***如果出现乱码: 执行*** `set names gbk;`             |                                                |
|        |              |                                                       |                                                |
| **删** | 删除数据     | `delete from 表名 where 条件；`                       |                                                |
|        | 删除全部     | `delete from 表名;`                                   |                                                |
|        |              |                                                       |                                                |
| **改** | 修改数据     | `update 表名 set 字段1名=值,字段2名=值 where 条件;`   | ***`null`不能用`=`要用`is`***                  |



---

### `DQL`语句

#### 基本语法

|        | 名称             | 格式                                    | 备注                  |
| ------ | ---------------- | --------------------------------------- | --------------------- |
| **查** | 查询表数据       | `select 字段信息 from 表名 where 条件;` |                       |
|        | 查询所有字段数据 | `select * from 表名;`                   | `*`代表查询的所有字段 |
|        | 添加别名         | `select 字段名 别名 from 表名;`         |                       |

#### 去重`distinct`

| 名称     | 格式                                |
| -------- | ----------------------------------- |
| 去重查询 | `select distinct 字段名 from 表名;` |

#### 条件查询`where`

| 名称     | 格式                                                 | 备注                       |
| -------- | ---------------------------------------------------- | -------------------------- |
| 条件查询 | `select * from 表名 where 字段名 判断运算符 值;` | ***判断运算符:`>` `<` `>=` `<=` `!=` `<>`*** |
| `and` `or` | `select * from 表名 where 字段名 判断符 值 逻辑符 字段名 判断符 值;` | ***逻辑运算符:<br />`and`:查询多个条件同时满足<br /> `or`:查询多个条件满足一个就行*** |
| `in` | `select * from 表名 where 字段名 in(值1,值2,值3);` |  |
|               | `select * from 表名 where 字段名 not in(值1,值2,值3);` |  |
| `between and` | `select * from 表名 where 字段名 between 值1 and 值2;` | ***查询某个字段的值在两者之间*** |

#### 排序查询`order by`

| 名称           | 格式                                                  | 备注             |
| -------------- | ----------------------------------------------------- | ---------------- |
| 升序查询       | `select * from 表名 order by 字段名 asc;`             | `asc`:默认，升序 |
| 降序查询       | `select * from 表名 order by 字段名 desc;`            | `desc`:降序      |
| 多字段排序查询 | `select * from 表名 order by 字段名,字段名 asc/desc;` |                  |

#### 模糊查询`like`

- *`%`代表0或多个未知字符*
- `_`*代表1个未知字符*

| 名称                         | 格式                                            |
| ---------------------------- | ----------------------------------------------- |
| 以x开头                      | `select * from 表名 where 字段名 like 'x%';`    |
| 以x结尾                      | `select * from 表名 where 字段名 like '%x';`    |
| 包含x                        | `select * from 表名 where 字段名 like '%x%';`   |
| 第二个字符是x                | `select * from 表名 where 字段名 like '_x%';`   |
| 倒数第三个字符是x            | `select * from 表名 where 字段名 like '%x_ _';` |
| 第二个字符是x最后一个字符是y | `select * from 表名 where 字段名 like '_x%y';`  |

#### 限制、分页查询`limit`

| 名称     | 格式                                                         |
| -------- | ------------------------------------------------------------ |
| 分页查询 | `select * from 表名 limit 跳过的条数,请求的条数;`            |
|          | `select * from 表名 order by 字段名 asc/desc limit 跳过的条数,请求的条数;` |

#### 聚合查询

##### `汇总函数`

| 名称             | 格式                                        |
| ---------------- | ------------------------------------------- |
| `sum`:求和       | `select sum(字段名) from 表名;`             |
| `count`:统计数量 | `select count(字段名/*) from 表名;`         |
| `max`:最大值     | `select max(字段名) from 表名;`             |
| `min`:最小值     | `select max(字段名),min(字段名) from 表名;` |
| `avg`:平均值     | `select avg(字段名) from 表名 where 条件;`  |

##### 分组查询`group by`

| 名称     | 格式                                                      |
| -------- | --------------------------------------------------------- |
| 分组查询 | `select 字段名1,avg(字段名2) from 表名 group by 字段名1;` |

##### `with`

- 表示对汇总之后的记录进行再次汇总

| 名称     | 格式                                                         |
| -------- | ------------------------------------------------------------ |
| with汇总 | `select 字段名1,count(1) from 表名 group by 字段名1 with rollup;` |

##### `having`

| 名称 | 格式                                                         |      |
| ---- | ------------------------------------------------------------ | ---- |
|      | `select 字段名1,avg(字段名2) from 表名 group by 字段名1 having avg(字段名2) 判断符 值;` |      |

#### 表连接

##### 内连接

| 名称   | 格式                                                         |
| ------ | ------------------------------------------------------------ |
| 内链接 | `select 表1.字段名1,表2.字段名1 from 表1,表2 where 表1.字段名2 判断符 表2.字段名2;` |

##### 外连接

| 名称     | 格式                                                         |
| -------- | ------------------------------------------------------------ |
| 左外连接 | `select 表1.字段名1,表2.字段名1 from 表1 left join 表2 on 表1.字段名2 判断符 表2.字段名2;` |
| 右外连接 | `select 表1.字段名1,表2.字段名1 from 表1 right join 表2 on 表1.字段名2 判断符 表2.字段名2;` |

#### 子查询

- 关键字:`in` `not in` `=` `!=`  `<>` `exists` `not exists`
- 在某些情况下，子查询可以转换为表连接

| 格式                                                         | 备注                                        |
| ------------------------------------------------------------ | ------------------------------------------- |
| `select 表1.* from 表1 where 字段名1 in (select 字段名1 from 表2);` |                                             |
| `select * from 表1 where 字段名1 = (select 字段名1 from 表2)；` | 子查询数量唯一可以用`=`替换`in`             |
| `select * from 表1 where 字段名1 = (select 字段名1 from 表2 limit 1,1);` | 子查询不唯一可以使用`limit`限制返回的记录数 |

#### 联合查询

- 关键字:`union` `union all`

| 名称        | 格式                                                         | 备注                                                 |
| ----------- | ------------------------------------------------------------ | ---------------------------------------------------- |
| `union all` | `select 字段名1 from 表1 union all select 字段名1 from 表2;` | `union all`是把结果集直接合并在一起                  |
| `union`     | `select 字段名1 from 表1 union select 字段名1 from 表2;`     | `union`是将`union all`后的结果进行一次`distinct`去重 |



---

### `DCL`语句

- `DCL`语句主要是管理数据库权限的时候使用，这类操作一般是DBA使用的，开发人员不会使用`DCL`语句。

---

### 主键约束 `Primary Key`

- 主键:数据唯一性的字段
- 约束:创建表时给表字段添加的限制
- 主键约束:**唯一且非空**

---

| 例子                                                      | 备注                 |
| --------------------------------------------------------- | -------------------- |
| `create table 表名(id int primary key,name varchar(10));` |                      |
| `insert into 表名 values(1,'aaa');`                       |                      |
| `insert into 表名 values(1,'bbb');`                       | ❌主键值重复          |
| `insert into 表名 values(null,'ccc');`                    | ❌主键值不能为 `null` |

---

### 自增

- 给字段添加自增后,赋值` null`则触发自增
- 自增规则:从**历史最大值**+1

| 例子                                                         | 备注      |
| ------------------------------------------------------------ | --------- |
| `create table 表名(id int primary key auto_increment,name varchar(10));` |           |
| `insert into 表名 values(null,'aaa');`                       | 主键值:1  |
| `insert into 表名 values(null,'bbb');`                       | 主键值:2  |
| `insert into 表名 values(10,'ccc');`                         | 主键值:10 |
| `insert into 表名  values(null,'ddd');`                      | 主键值:11 |
| `delete from 表名 where id>=10;`                             |           |
| `insert into 表名 values(null,'eee');`                       | 主键值:12 |

---

### 导入`*.sql`文件到`MySQL`中

- 把`emp.sql`文件放到某个盘的根目录 

- 在终端中执行

  `source d:/emp.sql;`

  `select * from emp;`   查看是否乱码   如果有乱码  执行  `set names gbk;`

### `is null` 和 `is not null`

- 如果查询字段的值为`null` 是`is null`  
- 不为`null` 则使用 `is not null`
  - 查询没有上级领导的员工信息
    - `select *  from emp where mgr is null;`
  - 查询有上级领导的员工姓名/工资和领导编号
    - `select ename,sal,mgr from emp where mgr is not null;`

