# Mysql Notes

## 常用命令

```mysql
// 连接
mysql -u username -puserpwd -h xxx.mysql.rds.aliyuncs.com
// 创建表
create database project_test;
// 修改列名
alter table customer change customercity customer_city VARCHAR(225);
// 导出表
mysqldump -d  -u root  -pve8uuuuu -h mysql.mysql.com dbname --column-statistics=0 > dump.sql
```

## 索引

建立索引会生成一个平衡树，主键的索引称为聚合索引，叶子节点可以取到数据，其他的非聚合索引取到的是主键

- key 或者index 可以建立索引，可以加快查询的速度，但是会降低写入的速度。
- unique key 索引是 key 的一种，只是约束了 key 的值必须是 unique 的
- primary key 索引是一种 unique key，只是约束了一个表里只能建立一个主键

### expression

### 查询

#### groupby

将表达式和aggregate_function分组查询之后，聚合在一起进行返回的这么一种操作

```sql
select expression1, expression2, aggregate_function (expression)
from tables
[where condition]
group by expression1, expression2
```

没有被包装进aggregate_function里的表达式必须出现在 groupby 条件里面;

1. 查询订单详情里取出所有产品，以及产品的数量
2. 查询订单详情里produce分类下所有的产品, 和produce分类下的订单总量
3. 从员工信息表里，查出来部门的名字以及最小工资

### like

用于对查询结果再进行一次 pattern 匹配

```sql
expression LIKE pattern [ ESCAPE 'escape_character' ]
```

pattern 有两种 `%` 代表0次或者任意次，`_`代表一次； `Not Like` 也是有效的；如果要转译，可以使用 \ 或者是 Escape 关键词.

### in

in 查询主要是为了减少在 `select`, `insert`, `delete`, `update` 时会多次使用 or 的问题

```sql
WHERE last_name IN ('Johnson', 'Anderson');
WHERE last_name = 'Johnson' OR last_name = 'Anderson'
```


sql structured query language
关系数据库返回的数据必须是二维关系表，列是字段，行是记录，关系数据必须以行为单位进行读写
一条sql语句由关键字、标名、列名组成，SQL 语句分为3类
- DDL(data definition lannguage) 数据定义，create/drop/alter
- DML(data Manipulation language) 数据操纵 select/insert/update/delete
- DCL(data control language) 数据控制 commit/roolback/grant/revoke
- 字符串'a', 日期'2010-10-10'，数字1，称为常数
- 一个书写习惯是：关键字大写

    ```sql
    CREATE DATABASE shop;
    CREATE TABLE Product(
    product_id  CHAR(4) NOT NULL,
    product_name VARCHAR(100) NOT NULL,
    sale_price INTERGER ,
    regist_date DATE ,
    PRIMARY KEY (product_id)
    )
    ```

- CHAR 类型是固定长度的类型，VARCHAR 是可变长度
- *约束* 是除了数据类型外，对数据对限制和条件
- key：在指定特定数据时使用的列的组合，主键就是可以特定一行数据的列

    ```sql
    DROP TABLE Product;
    ALTER TABLE Product ADD COLUMN  Product_name_pinyin VARCHAR(100); // 给表增加一列
    ALTER TABLE Product DROP COLUMN product_name_pinyin; // 给表删除一列
    INSERT INTO Product VALUES('001', 'name', 100, '2020-10-10'); // ??
    // 重命名一个表
    ```

- select as

    ```sql
    SELECT product_id, product_name, sale_price AS price // 可以使用 as 设定别名，可以中文，用双引号扩起来
    FROM product;
    ```

- select 常数查询

    ```sql
    SELECT '商品' AS string, 38 AS number, product_id, product_name // 常数会填充每一行
    FROM product;
    ```

- select distint 删除重复数据，如果使用了多个字段，那么同时满足两个条件的重复行会被移除

    ```sql
    SELECT DISINCT product_type // null 也算是一条数据
    FROM product;
    ```