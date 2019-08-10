#### 1.在mybatis全局配置文件中配置如下的标签
```
//databaseIdProvider:用于支持多数据库厂商的配置标签
<databaseIdProvider type="DB_VENDOR">
    <!-- 为不同的数据库厂商起别名 name:数据库厂商标识 value：别名-->
    <property name="MySQL" value="mysql"/>
    <property name="Oracle" value="oracle"/>
    <property name="SQL Server" value="sqlserver"/>
</databaseIdProvider>
```

#### 2.在mybatis的SQL配置文件中为相应的SQL语句添加databaseId属性即可
```
<select id="selectByKey" resultType="结果类型全类名" databaseId="mysql">
   select * from emp where id = #{id}
</select>
```
