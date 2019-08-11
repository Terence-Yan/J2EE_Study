#### 1.全局setting设置
```
1.autoMappingBehavior默认是PARTIAL，开启自动映射的功能。唯一的要求是数据库表的列名要与
  JavaBean对象的属性名一致(不区分大小写)。
2.如果autoMappingBehavior设置为null，则会取消自动映射。
3.数据库表字段命名规范，POJO属性符合驼峰命名法，如 HOME_TELNUM -> homeTelnum，此时只需开启
  自动驼峰命名规则映射功能即可：
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

#### 3.使用resultMap解决属性也是Java对象的映射
```
场景：查询的返回值类型是Employee对象，其dept属性也是Java对象——Department对象
方法一：使用级联属性封装结果集
<resultMap type="com.it.Employee" id="MyEmp">
   <id column="id" property="id"/>
   <result column="last_name" property="lastName"/>
   <result column="gender" property="gender"/>
   <result column="did" property="dept.id"/> // 级联属性
   <result column="dept_name" property="dept.name"/> // 级联属性
</resultMap>

方法二：使用association标签
<resultMap type="com.it.Employee" id="MyEmp">
   <id column="id" property="id"/>
   <result column="last_name" property="lastName"/>
   <result column="gender" property="gender"/>
   // property:指明需要进行关联映射的Employee对象的属性名称
   // javaType:指明该属性的Java类型
   <association property="dept" javaType="com.it.Department"> 
      <id column="did" property="id"/>
      <result column="dept_name" property="name"/>
   </association> 
</resultMap>

sql语句的声明：
 <select id="getEmpById" resultMap="MyEmp">
   select e.id id, e.last_name last_name, e.gender gender,
   d.id did, d.name dept_name from tbl_emp e, tbl_dept d where ...
 </select>
 
 方法三：使用association标签进行分步查询
 <resultMap type="com.it.Employee" id="MyEmp">
   <id column="id" property="id"/>
   <result column="last_name" property="lastName"/>
   <result column="gender" property="gender"/>
   // property:指明需要进行关联映射的Employee对象的属性名称
   // select:指明该属性是通过select属性指定的方法查询出的结果
   // column:指明将(第一次查询出的)哪一列的值传给该查询方法
   // 流程：使用select指定的方法(传入column指定的这列参数的值)查出结果，并封装给property指定的属性
   <association property="dept" select="com.it.DepartmentMapper.getDeptById" column="did"> 
   </association> 
</resultMap>
sql语句的声明：
 <select id="getEmpById" resultMap="MyEmp">
   select * from tbl_emp where id=#{id}
 </select>
 
 使用association标签进行分步查询时，可以使用mybatis的懒(延迟)加载机制：
  1.不使用懒加载机制，每次查询Employee对象时，其属性dept(即第二次查询的Department对象)会同时查询出来；
  2.使用懒加载机制，部门信息(即Department对象)只有在需要的时候才会去查询(即才会执行第二次查询)；
  3.使用懒加载机制，需要在全局配置中添加如下的属性：
    // 显示指定每个我们需要更改的配置的值，即使它是默认的，以防止版本更迭带来的问题
    <setting name="lazyLoadingEnabled" value="true"/>
    <setting name="aggressiveLazyLoading" value="false"/>
```









