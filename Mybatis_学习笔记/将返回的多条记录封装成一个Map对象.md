#### 1.在mybatis中，对于返回多条记录的查询，会默认封装成一个List集合，但通过添加注解还可以将其封装成一个Map集合
```
查询接口方法声明：
@MapKey("stuId")  // 告诉mybatis，在将结果封装成Map对象时，使用JavaBean对象(即Student对象)的哪个属性作为主键
public Map<Integer, Student> getStuInfoByScore(Integer score);

sql语句的声明：
// resultType:指明返回结果集中一条记录所对应的Java映射类型
<select id="getStudent" resultType="com.it.Student" >
  ...
</select>
```
