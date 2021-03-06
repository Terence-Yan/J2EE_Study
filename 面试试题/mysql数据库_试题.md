#### 1.一条更新SQL语句执行较慢，可能的原因有哪些？
```
1.明确慢的情形：偶尔变慢；一直很慢
2.偶尔变慢的原因有：
  (1).数据库刷新脏页，如 redo log写满了需要同步到磁盘；
  (2).执行的时候，遇到了锁，如表锁、行锁。
3.一直很慢的原因有：
  (1).没有创建索引；
  (2).创建了索引，但未被使用；
  (3).数据库选择了不恰当的索引。
```

###### 参看文章
* <a href="https://mp.weixin.qq.com/s/llq5_hzqCKXbcjNtvOL5XA" target="_blank">一条SQL语句执行得很慢的原因有哪些？</a>

#### 2.分页查询时，使用limit语句对查询性能有影响吗？如何优化？
分页查询时，使用limit语句，数据量越大，越往后，查询性能越差。
###### 参看文章
* <a href="https://mp.weixin.qq.com/s/8aU3B3nJWR8y8ljBrFNaWw" target="_blank">MySQL 百万级数据量分页查询方法及其优化</a>

#### 3.索引的分类有哪些？
###### 根据存储索引的数据结构的不同，可以将索引分为：
* B-Tree索引
* 哈希索引
* 空间数据索引(R-Tree)
* 全文索引等
###### 根据创建索引时包含的列的多少，又可以将索引分为：
* 单值索引：一个索引只包含单个列，一个表可以有多个单列索引
* 唯一索引：索引列的值必须唯一，但允许有空值
* 复合索引：一个索引包含多个列

#### 4.导致索引失效的因素有哪些？
* 如果查询中的列不是独立的，则MySQL就不会使用索引。
```
“独立的列”是指索引列不能是表达式的一部分，也不能是函数的参数。
```

#### 5.什么是聚簇索引？
聚簇索引并不是一种单独的索引类型，而是一种数据存储方式。具体的细节依赖于其实现方式，但InnoDB的聚簇索引实际上是**在同一个结构中保存了B-Tree索引和数据行**。

#### 6.使用复合索引时，什么情况下索引会失效？复合索引的使用准则是什么？
由于MySQL使用的是B-Tree索引，所以在使用复合索引时，一定要遵循**最左前缀法则**：
```
例：
表名：student
表字段：  name, stuNo, age, score,...
复合索引：(name, age, score)
```
* 全职匹配：和索引中所有的列进行匹配，且索引字段的顺序必须一致
```
例：select * from student where name='Tom' and age=16 and score=468;
```
* 匹配最左前缀
```
例：select * from student where name='Tom';
```
* 匹配列前缀
```
例：select * from student where name='T%';
```
* 精确匹配某一列并范围匹配另一列
```
例：select * from student where name='Tom' and age>=16;
```
* 只访问索引的查询
```
例：select name, age from student where name='T%' and age=18;
```
###### 以下情形会导致索引失效：
* 不是按照索引的最左列开始查找，则无法使用索引
```
例：select * from student where age=18 and score>420;
例：select * from student where age=18;
例：select * from student where name='%ey';
```
* 索引中被跳过的列以后的索引无法使用
```
例：select * from student where name='To%' and score>420;(只能索引name列)
```
* 查询中存在某个列的范围查询，则其后面的索引列都无法使用
```
例：select * from student where name='To%' and age>17 and score>420;(score列索引失效)
```

#### 7.什么是覆盖索引？
如果一个索引包含(或者说覆盖)所有需要查询的字段的值，我们就称之为“**覆盖索引**”。 覆盖索引是非常有用的工具，能够极大地提高性能。

#### 8.索引的优点是什么？或者说为什么索引能够提升查询性能？
* 索引大大减少了服务器需要扫描的数据量。
* 索引可以帮助服务器避免排序和临时表。
* 索引可以将随机I/O变为顺序I/O。

#### 9.索引一定能提升数据表的查询性能。

#### 10.如何评价一个索引是否适合某个查询？
```
Lahdenmaki 和 Leach 的“三星系统”(three-star system):
(1).索引将相关的记录放到一起则获得一星；
(2).如果索引中的数据顺序和查找中的排列顺序一致则获得二星；
(3).如果索引中的列包含了查询中需要的全部列则获得“三星”。
```

#### 11.创建高效索引的一些最佳实践
* 尽可能将需要做范围查询的列放到(多列)索引的最后，以便优化器能使用尽可能多的索引列。

#### 12.如何检查索引是否损坏？
```
损坏的索引会导致查询返回错误的结果或者莫须有的主键冲突等问题，严重时甚至会导致数据库的崩溃。如果遇到了古怪的
问题——例如一些不应该发生的错误——可以尝试使用 CHECK TABLE 来检查是否发生了表损坏(注意有些存储引擎不支持该命令)。
CHECK TABLE通常能够找出大多数表和索引的错误。此时，可以尝试使用 REPAIR TABLE来修复损坏的表，同样不是所有的
存储引擎都支持该命令。如果存储引擎不支持，也可通过一个不做任何操作的 ALTER 操作来重建表，例如修改表的存储引擎
为当前的引擎。下面是一个针对InnoDB表的例子：
    ALTER TABLE innodb_tbl ENGINE=INNODB;
```

#### 13.sql语句性能下降的主要表现是什么？
执行时间长、等待时间长

#### 14.造成sql语句性能下降的常见因素有哪些？
* 查询语句写的不好
* 索引失效
* 关联查询涉及较多的join操作(设计缺陷或不得已的需求)
* 服务器参数或mysql配置参数设置不合理(缓冲、线程数等)

#### 15.索引的优势与劣势
* 优势：
* 类似于图书馆创建书目索引，提高数据检索的效率，降低数据库的IO成本
* 通过索引列对数据进行排序，降低数据排序的成本，降低了CPU的消耗
* 劣势：
* 实际上索引也是一张表，该表保存了主键与索引字段，并指向实体表的记录，所以索引列也要占据存储空间
* 虽然索引大大提高了查询速度，同时却会降低更新表的速度，如INSERT、UPDATE和DELETE操作。因为更新表的时候，MySQL不仅要保存数据，还要保存索引文件每次更新添加了索引列的字段，都会调整因为更新所带来的键值变化后的索引信息
* 索引只是提高效率的一个因素，如果MySQL有大数据量的表，就需要花时间研究建立高效的索引，或优化查询语句

#### 16.造成MySQL服务器性能瓶颈的常见因素
* CPU：CPU高负荷运行一般发生在数据装入内存或从磁盘上读取数据的时候
* IO：磁盘I/O发生在装入数据远大于内存容量的时候
* 服务器硬件的性能瓶颈：top、free、iostat和vmstat来查看系统的性能状态信息

#### 17.什么情况下需要创建索引？
* 主键自动建立唯一索引
* 频繁作为查询条件的字段应该创建索引
* 查询中与其他表关联的字段，外键关系建立索引
* 频繁更新的字段不适合创建索引：因为每次更新不单是更新了记录还会更新索引，加重了IO负担
* where条件里面用不到的字段不应该创建索引
* 查询中排序的字段应该创建索引，排序字段若通过索引访问将大大提高访问速度
* 查询中统计或者分组字段不应该创建索引

#### 18.什么情况下不需要创建索引？
* 表记录数较少
* 经常更新的表
* 数据重复且分布平均的字段，因此应该只为最经常查询和最经常排序的数据创建索引。如果某个数据列包含许多重复的内容，为它建立索引就没有太大的实际效果。

#### 19.mysql索引的分类
* 单值索引：一个索引只包含单个列，一个表可以有多个单列索引
* 唯一索引：索引列的值必须唯一，但允许有空值
* 复合索引：一个索引包含多个列

#### 20.数据库事务的四要素——ACID
* A——原子性(Atomicity)，一个事务中的所有操作，要么全部成功，要么全部失败
* C——一致性(Consistency)，一个事务执行之前和执行之后都必须处于一致性状态，或者说最终的数据状态要符合逻辑。
* I——隔离性(Isolation)，当多个用户并发访问数据库时，比如操作同一张表时，数据库为每一个用户开启的事务，不能被其他事务的操作所干扰，多个并发事务
之间要相互隔离。
* D——持久性(Durability)，在事务完成以后，该事务对数据库所作的更改便持久的保存在数据库之中，并不会被回滚。

#### 21.数据库事务的隔离级别
事务隔离级别|是否允许脏读|是否允许不可重复读|是否允许幻读
-----------|:--------------:|:--------------:|:--------------:
read-uncommitted(读未提交)|是|是|是
read-committed(不可重复读)|否|是|是
repeatable-read(可重复读)|否|否|是  
serializable(串行化)|否|否|否  
* mysql默认的事务隔离级别为 repeatable-read  </br>
* 幻读：事务A在对表T进行新增或删除操作(事务已提交)，事务B连续两次对表T进行读取操作，第二次读取数据时，出现了新增的记录或记录被删除的现象，称为幻读。
* 不可重复读：事务A在对表T的某一行记录进行修改操作(事务已提交)，事务B连续两次对表T的该行进行读取操作，第二次读取数据时，出现了该行数据与第一次
不一致的现象，称为不可重复读。
* 脏读：事务A在对表T的某一行记录进行修改操作(事务未提交)，事务B在对表T的该行进行读取操作，并且读取的是事务A修改且未提交的数据，之后事务A又进行了
回滚操作，此时事务B读取的数据是无效的，称为脏读。

#### 22.InnoDB 的索引模型
* 在 InnoDB 中，表都是根据**主键顺序**以索引的形式存放的，这种存储方式的表称为索引组织表。InnoDB 使用了 **B+ 树**索引模型，
所以数据都是存储在B+树中。
* 根据叶子节点存储的值的类型，索引类型分为**主键索引**和**非主键索引**：
```
主键索引的叶子节点存储的是整行数据。在 InnoDB 里，主键索引也被称为聚簇索引(clustered index)。
非主键索引的叶子节点存储的是主键的值。在 InnoDB 里，非主键索引也被称为二级索引(secondary index)。
```

#### 23.对于InnoDB引擎，谈谈使用业务字段做主键与自增主键的优劣
* 对于自增主键，每次插入一条新记录，都是追加操作，都不涉及到挪动其他记录，也不会触发叶子节点的分裂。
* 使用带有业务逻辑的字段做主键，则往往不容易保证有序插入，这样写数据成本相对较高。
* 主键长度越小，普通索引的叶子节点就越小，普通索引占用的存储空间也就越小。
* 所以，从性能和存储空间方面考量，自增主键通常是更合理的选择。





















