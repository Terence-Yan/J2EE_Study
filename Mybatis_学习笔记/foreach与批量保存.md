#### 1.MySQL使用foreach标签进行批量保存
```
◆ MYSQL下批量保存：由于mysql支持values(),(),...语法，可以使用foreach标签进行遍历
<insert id="addEmps">
    insert into tbl_emp(d_id, last_name, email, gender)
    values
    <foreach colleaction="emps" item="emp" separator=",">
      (#{emp.dept.id}, #{emp.lastName}, #{emp.email}, #{emp.gender})
    </foreach>
</insert>

方式二：（这种方式需要设置数据库的连接属性）
修改数据源配置文件如下：
------------------------------------------------------------
...
jdbc.url=jdbc:mysql://localhost:3306/myDbName?allowMultiQueries=true
...
------------------------------------------------------------
// 这种分号分割多个sql也可用于其他的批量操作
<insert id="addEmps">
    <foreach colleaction="emps" item="emp" separator=";">
       insert into tbl_emp(d_id, last_name, email, gender) 
       values(#{emp.dept.id}, #{emp.lastName}, #{emp.email}, #{emp.gender})
    </foreach>
</insert>
```

#### 2.Oracle使用foreach标签进行批量保存
```
Oracle数据库不支持 “values(),(),...”语法
<insert id="addEmps">
    <foreach colleaction="emps" item="emp" separator=";">
       insert into tbl_emp(d_id, last_name, email, gender) 
       values(#{emp.dept.id}, #{emp.lastName}, #{emp.email}, #{emp.gender})
    </foreach>
</insert>
```































