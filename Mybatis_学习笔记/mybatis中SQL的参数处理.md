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
```
