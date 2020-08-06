## 主键约束 `Primary Key`

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

## 自增

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

