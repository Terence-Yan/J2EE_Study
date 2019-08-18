#### 1.mybatis的默认的两个内置参数
```
1.不只是方法传递过来的参数可以被用来取值、判断;
2.mybatis默认还有两个内置参数：
  _parameter : 代表整个参数
      如果传递的是单个参数，_parameter就是这个参数；
      如果传递的是多个参数，mybatis会将多个参数封装成一个map, _parameter代表的就是这个map;
  _databaseId : 如果配置 databaseIdProvider 标签，_databaseId就代表当前数据库的别名
  
  查询接口声明：
  public List<Employee> getEmps(Employee emp);
  
  <select id="getEmpWithInnerParameter" resultType="com.it.Employee">
      <if test="_databaseId == 'mysql'">
        select * from tbl_emp
        <if test="_parameter != null">
           where last_name = #{lastName(或者_parameter.lastName)} // 此时_parameter就代表传递的 emp 参数
        </if>
      </if>
      <if test="_databaseId=='oracle'">
        select * from employee
      </if>
  </select>
```
