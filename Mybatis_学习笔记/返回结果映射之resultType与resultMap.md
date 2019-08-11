#### 1.全局setting设置
```
1.autoMappingBehavior默认是PARTIAL，开启自动映射的功能。唯一的要求是数据库表的列名要与JavaBean对象的属性名一致(不区分大小写)。
2.如果autoMappingBehavior设置为null，则会取消自动映射。
3.数据库表字段命名规范，POJO属性符合驼峰命名法，如 HOME_TELNUM -> homeTelnum，此时只需开启自动驼峰命名规则映射功能即可：
  mapUnderscoreToCamelCase=true
```

#### 2.resultMap——自定义结果集映射规则，实现高级结果集映射
```
  // type:指定结果集映射的Java对象类型，即将返回的一条记录映射为一个该类型的Java对象
  // id:唯一标识符，用于引用该resultMap
  <resultMap type="com.it.Student" id="MyStu">
     // column:指定要映射的数据库表的列名
     // property:指定映射结果的JavaBean对象的属性名
     // 即将数据表中的id字段的值映射为Student对象的id属性值
     <id column="id" property="id"/>  // id标签：指定主键列的封装规则，使用该标签定义主键会有优化
     <result column="stu_name" property="stuName"/> // result标签：定义普通列的映射规则
     ...
     // 如果其他列不指定映射规则，则采用默认规则自动映射：推荐使用resultMap时，将所有列的映射规则都进行定义
  </resultMap>
  // 自定义结果集映射规则
  // 在sql语句声明时，resultMap与resultType属性只能有一个
  <select id="getXXX" resultMap="MyStu">
  ...
  </select>
```
