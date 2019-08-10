#### 1.单个参数
```
mybatis不做特殊处理：
#{传入的参数名(此时不会以此参数名进行映射取值，只是相当于一个占位符，写什么都可以)}：取出参数值
<select ...>
  select * from emp where id=#{id}
</select>
```

#### 2.多个参数
```
1.mybatis会做特殊处理,会将传入的多个参数封装成一个Map,其中Map的key是:
  param1 ... paramN，与传入的多个参数按传入顺序对应
  或者参数的索引,从0开始
  key所对应的value才是传入的参数值
#{paramX或参数的索引}：取出参数值
<select ...>
  select * from emp where id=#{param1} and name=#{param2}
</select>
 对应的接口方法声明：
 public Bean getBeanByIdAndName(Integer id, String name)
2.命名参数：明确指定封装参数是Map的key
  多个参数会被封装成一个Map对象：
    key : 使用@Param注解指定的值
    value : 传入的参数值
 SQL示例：
 <select ...>
  select * from emp where id=#{id} and name=#{name}
 </select>
 对应的接口方法声明：
 public Bean getBeanByIdAndName(@Param("id")Integer id, @Param("name")String name)
```

#### 3.多个参数——传入POJO或Map对象
```
SQL中获取传入参数值的方式：#{POJO的属性名或Map对象的key}
如果多个参数不是业务模型中的数据，并且要经常使用，推荐编写一个TO(Transfer Object)数据传输对象
```

#### 4.#{}与${}的区别
```
#{}：是以预编译(PrepareStatement)的形式，将参数设置到sql语句中，可以防止sql注入；
${}：将参数值直接拼接在sql语句中，会有安全问题；
一般情况下，我们取参数值都应该使用#{}；
在原生jdbc不支持占位符的地方，就可以使用${}进行取值：
比如按照年份进行分表时的查询 select * from ${year}_salary where ... 。
```

#### 5.#{}的更多用法
```
1.使用#{}对参数进行取值时，可以添加一些约束或规则：javaType、jdbcType、mode(存储过程)、numericScale、resultMap、
typeHandler、jdbcTypeName、expression(未来准备支持的功能)等。
2.jdbcType通常需要在某些情况下被设置：在数据为null时，有些数据库可能无法识别mybatis对null值的默认处理，比如Oracle(报错)：
  JdbcType OTHER：无效的类型——因为mybatis对所有的null都会映射为原生JDBC的OTHER类型，Oracle不能正确处理。
3.由于全局配置中，jdbcTypeForNull=OTHER, Oracle不支持。此问题有两种解决方法：
  1).参数取值时，设置数据库映射类型：#{name, jdbcType=NULL};
  2).修改全局配置为：jdbcTypeForNull=NULL.
  <setting name="jdbcTypeForNull" value="NULL"/>
```


