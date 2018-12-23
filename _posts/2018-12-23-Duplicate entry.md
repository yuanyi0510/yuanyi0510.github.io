---
layout: post
title: "Hibernate错误"
date: 2018-12-23
tag: 错误日记
---

在运行hibernate多对多关系时一直报下图的错误
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181223144115152.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l1YW55aTA1MDE=,size_16,color_FFFFFF,t_70) 

### 代码
实体类

```
public class SysUser {
    private long userId;
    private String userCode;
    private String userName;
    private String userPassword;
    private String userState;

    private Set<SysRole> roles=new HashSet<>();

    public Set<SysRole> getRoles() {
        return roles;
    }
    ……
```

```
public class SysRole {
    private long roleId;
    private String roleName;
    private String roleMemo;

    private Set<SysUser> users=new HashSet<>();

    public Set<SysUser> getUsers() {
        return users;
    }
    ……
```
实体类映射文件

```
<hibernate-mapping>

    <class name="com.hibernate.domain.SysUser" table="sys_user" schema="yy-visualization">
        <id name="userId" column="user_id">
            <generator class="native"></generator>
        </id>
        <property name="userCode" column="user_code"/>
        <property name="userName" column="user_name"/>
        <property name="userPassword" column="user_password"/>
        <property name="userState" column="user_state"/>

        <!--
        name：关联的另一方的集合属性名称
        table：中间表的名称
        -->
        <set name="roles" table="sys_user_role">
            <!--column：当前对象在中件表中的外键名称-->
            <key column="user_id"></key>
            <!--
            class：关联另一方的类全路径
            column：关联另一方在中件表的外键名称
            -->
            <many-to-many class="com.hibernate.domain.SysRole"
                          column="role_id"></many-to-many>
        </set>
    </class>
</hibernate-mapping>
```

```
<hibernate-mapping>

    <class name="com.hibernate.domain.SysRole" table="sys_role" schema="yy-visualization">
        <id name="roleId" column="role_id">
            <generator class="identity"></generator>
        </id>
        <property name="roleName" column="role_name"/>
        <property name="roleMemo" column="role_memo"/>

        <set name="users" table="sys_user_role">
            <key column="role_id"></key>
            <many-to-many column="user_id"
                          class="com.hibernate.domain.SysUser"></many-to-many>
        </set>
    </class>
</hibernate-mapping>
```
测试类

```
@Test
    public void demo03(){
        Configuration configuration = new Configuration().configure();
        SessionFactory sessionFactory = configuration.buildSessionFactory();
        Session session = sessionFactory.openSession();

        Transaction tx=session.beginTransaction();
        SysUser u1=new SysUser();
        u1.setUserCode("1231323213");
        u1.setUserPassword("123");
        u1.setUserState("1");
        u1.setUserName("敖哈哈");
        SysUser u2=new SysUser();
        u2.setUserCode("435454");
        u2.setUserName("袁嘻嘻");
        u2.setUserPassword("123");
        u2.setUserState("1");

        SysRole r1=new SysRole();
        r1.setRoleName("男朋友");
        SysRole r2=new SysRole();
        r2.setRoleName("女朋友");
        SysRole r3=new SysRole();
        r3.setRoleName("室友");

        u1.getRoles().add(r1);
        u1.getRoles().add(r3);
        u2.getRoles().add(r2);
        u2.getRoles().add(r3);

        r1.getUsers().add(u1);
        r2.getUsers().add(u2);
        r3.getUsers().add(u1);
        r3.getUsers().add(u2);
        session.save(u1);
        session.save(u2);
        session.save(r1);
        session.save(r2);
        session.save(r3);
        tx.commit();
        session.close();
    }
```

### 错误原因猜测：
hibernate在save的时候会产生多余的sql语句，可能多余的sql语句导致数据表中主键重复
### 解决方法
在多对多关系中，找到一方放弃关系维护，我这里是让role放弃
在user的映射文件中的set标签，添加inverse属性
```
 <set name="roles" table="sys_user_role" inverse="true">
            <!--column：当前对象在中件表中的外键名称-->
            <key column="user_id"></key>
            <!--
            class：关联另一方的类全路径
            column：关联另一方在中件表的外键名称
            -->
            <many-to-many class="com.hibernate.domain.SysRole"
                          column="role_id"></many-to-many>
        </set>
```

