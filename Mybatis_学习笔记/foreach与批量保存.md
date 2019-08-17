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
-------------------------------------------------------------------
1.Oracle数据库不支持 “values(),(),...”语法
2.Oracle支持的批量插入方式：
  1).多个insert语句放在begin-end语句块中：
     begin
         insert into tbl_emp(d_id, last_name, email, gender) 
         values("1", "Li", "123@123.com", "1");
         insert into tbl_emp(d_id, last_name, email, gender) 
         values("2", "Sn", "234@123.com", "0");
      end;
   2).利用中间表：
      insert into tbl_emp(d_id, last_name, email, gender)
          select tbl_emp_seq.nextval, lastName, email from(
             selcet 'test_01' lastName, 'test_01_0' email from dual
             union
             selcet 'test_02' lastName, 'test_02_0' email from dual
             union
             selcet 'test_03' lastName, 'test_03_0' email from dual
          )
          
-------------------------------------------------------------------
<insert id="addEmps" databaseId="oracle">
    begin
    <foreach colleaction="emps" item="emp">
       insert into tbl_emp(d_id, last_name, email, gender) 
       values(#{emp.dept.id}, #{emp.lastName}, #{emp.email}, #{emp.gender});
    </foreach>
    end;
</insert>

<insert id="addEmps" databaseId="oracle">
    insert into tbl_emp(d_id, last_name, email, gender)
        select did, lastName, email, gender from(
              <foreach colleaction="emps" item="emp" separator="union">
                 selcet #{emp.dept.id} did, #{emp.lastName} lastName, #{emp.email} email, #{emp.gender} gender from dual
              </foreach>
          )
     或者
     insert into tbl_emp(d_id, last_name, email, gender)
         <foreach colleaction="emps" item="emp" separator="union" 
                  open="select did, lastName, email, gender from(" close=")">
              selcet #{emp.dept.id} did, #{emp.lastName} lastName, #{emp.email} email, #{emp.gender} gender from dual
         </foreach>
</insert>
```































