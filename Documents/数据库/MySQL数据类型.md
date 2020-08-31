## `MySQL`数据类型

- 数值类型
- 日期和时间类型
- 字符串类型
---

### 数值类型

- 严格数值类型
- 近似数值数据类型
- 扩展数据类型

#### 整数类型

- `int(m)`

  - `m`代表显示长度
  - 需要结合`zerofill`使用

- `bitint(m)`

  - 大整型
  - 等效`Java`中的`long`

  ```mysql
  create table t1(age int(10) zerofill);
  insert into t1 values(186);
  select*from t1;
  ```

#### 浮点数类型

- `double(m,d)`

  - `m`代表总长度，`d`代表小数长度

  ```mysql
  28.234
  m=5
  d=3
  ```

- `decimail(m,d)`
  - 超高精度浮点数
  - 只有涉及到超高精度运算时使用

### 日期和时间类型

- `date`

  - 只能保存年月日

- `time`

  - 只能保存时分秒

- `datetime`

  - 年月日时分秒
  - 默认值为`null`
  - 最大`999-12-31`

- `timestamp`时间戳

  - 年月日时分秒
  - 默认值为当前系统时间`now()`
  - 最大`2038-1-19`

  ```mysql
  create table t_date(t1 date,t2 time,t3 datetime,t4 timestamp);
  insert into t_date values('2020-7-14',null,null,now());
  insert into t_date values(null,'17:44:20','2010-10-10 10:10:10',now());
  ```

  

### 字符串类型

- `char(m)`
  - 固定长度字符串
  - 例`m=10` 存入`"abc"` 占`10`
  - 执行效率略高于可变长度
  - 最大长度`255`
- `varchar(m)`
  - 可变长度字符串
  - 例`m=10` 存入`"abc"` 占`3`
  - 节省空间
  - 最大长度`65535`
  - 建议保存字符长度为`255`以下的内容
- `text(m)`
  - 可变长度字符串
  - 最大长度`65535`
  - 建议保存长度大于`255`的字符串