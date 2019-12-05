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

#### 5.什么是聚簇索引？
聚簇索引并不是一种单独的索引类型，而是一种数据存储方式。具体的细节依赖于其实现方式，但InnoDB的聚簇索引实际上是**在同一个结构中保存了B-Tree索引和数据行**。

#### 6.使用复合索引时，什么情况下索引会失效？复合索引的使用准则是什么？
由于MySQL使用的是B-Tree索引，所以在使用复合索引时，一定要遵循**最佳左前缀法则**：
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
**如果一个索引包含(或者说覆盖)所有需要查询的字段的值，我们就称之为“覆盖索引”。**覆盖索引是非常有用的工具，能够极大地提高性能。

#### 8.索引的优点是什么？或者说为什么索引能够提升查询性能？
* 索引大大减少了服务器需要扫描的数据量。
* 索引可以帮助服务器避免排序和临时表。
* 索引可以将随机I/O变为顺序I/O。

#### 9.索引一定能提升数据表的查询性能。


























