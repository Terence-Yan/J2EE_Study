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

#### 4.resultMap中的collection标签
```
public class Department{
    private Integer id;
    private String deptName;
    private List<Employee> emps;
    ...
}

public interface DepartmentMapper{
    public Department getDeptById(Integer id);
    ...
}

需求：查询部门的时候将其所有的员工信息也查询出来
DepartmentMapper.xml文件:
...
// collection嵌套结果集的方式，定义关联的集合类型元素的封装规则
<resultMap type="com.it.Department" id="MyDept">
   <id column="did" property="id"/>
   <result column="dept_name" property="deptName"/>
   // collection:定义集合类型属性的封装规则
   // ofType:定义集合中元素的类型
   <collection property="emps" ofType="com.it.Employee">  
       // 定义集合中元素的封装规则
       <id column="eid" property="id"/>
       <result column="last_name" property="lastName"/>
       <result column="email" property="email"/>
       <result column="gender" property="gender"/>
   </collection>  
</resultMap>

<select id="getDeptById" resultMap="MyDept">
   select d.id did, d.dept_name dept_name, 
          e.id eid, e.last_name last_name, e.email email, e.gender gender
   from tbl_dept d
   left join tbl_emp e
   on d.id = e.d_id
   where id=#{id}
</select>

// 方式二：分步查询

public interface DepartmentMapper{
    public Department getDeptByIdInStep(Integer id);
    ...
}

public interface EmployeeMapper{
    public List<Employee> getEmpsByDeptId(Integer deptId);
    ...
}

EmployeeMapper.xml文件：
<select id="getEmpsByDeptId" resultType="com.it.Employee">
   select *
   from tbl_emp
   where d_id=#{deptId}
</select>

<resultMap type="com.it.Department" id="MyDeptInStep">
   <id column="id" property="id"/>
   <result column="dept_name" property="deptName"/>
   // collection:定义集合类型属性的封装规则
   // ofType:定义集合中元素的类型
   // column:指明将(第一次查询出的)哪一列的值传给该查询方法
   <collection property="emps" select="com.it.dao.EmployeeMapper.getEmpsByDeptId" 
               cloumn="id" fetchType="eager">  
       // 定义集合中元素的封装规则
       <id column="eid" property="id"/>
       <result column="last_name" property="lastName"/>
       <result column="email" property="email"/>
       <result column="gender" property="gender"/>
   </collection>  
</resultMap>

// ※如何给collection 与 association标签的cloumn属性传递多个值
   将多个值封装成map格式即可：column="{key1=column1,key2=column2,...}"
   如上例还可以写成：cloumn="{deptId=id}",其中deptId是方法形参的名称，id是列名，
   表示将第一次查询出的“id”列的值赋值给该查询方法参数中的名称为deptId的参数
// ※collection 与 association标签的 fetchType 属性
   fetchType="lazy":表示使用延迟加载，默认值
   fetchType="eager":表示不使用延迟加载，立即加载
   
<select id="getDeptByIdInStep" resultMap="MyDeptInStep">
   select id, dept_name
   from tbl_dept
   where id=#{id}
</select>
```

#### 5.resultMap中的 discriminator 标签
```
discriminator：鉴别器，mybatis可以使用该标签判断某列的值，然后根据不同的列值改变封装行为

需求：封装Employee
    如果查出来的是女生，就查询其部门信息，否则不查询；
    如果是男生，把last_name列的值赋给email属性
<resultMap type="com.it.Employee" id="MyEmp">
   <id column="id" property="id"/>
   <result column="last_name" property="lastName"/>
   <result column="email" property="email"/>
   <result column="gender" property="gender"/>
   // column:指定要进行判定的列名
   // javaType:列值所对应的java类型
   <discriminator javaType="string" column="gender">
       <case value="0" resultType="com.it.Employee">
           <association property="dept" select="com.it.DepartmentMapper.getDeptById" column="did"> 
           </association> 
       </case>
       <case value="1" resultType="com.it.Employee">  // resultType:指定封装结果的类型，或者使用resultMap，不能缺省
           <id column="id" property="id"/>
           <result column="last_name" property="lastName"/>
           <result column="last_name" property="email"/>
           <result column="gender" property="gender"/> 
       </case>
   </discriminator>
</resultMap>
```






