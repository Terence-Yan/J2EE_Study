#### 1.bind标签
```
将OGNL表达式的值绑定到一个变量上，便于后面引用这个变量的值

<select id="getEmpWithInnerParameter" resultType="com.it.Employee">
      // value : ONGL表达式的结果
      <bind name="_lastName" value="'%' + lastName + '%'"/>
      <if test="_databaseId == 'mysql'">
        select * from tbl_emp
        <if test="_parameter != null">
           where last_name = #{_lastName} 
        </if>
      </if>
      <if test="_databaseId=='oracle'">
        select * from employee
      </if>
  </select>
```
