---
title: SpingData JPA
date: 2022-02-24 16:32:37
tags: 笔记
---

# 一、SpirngBoot+JPA整合流程

**1、新建project**

**2、引入依赖**

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>runtime</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <optional>true</optional>
    </dependency>
</dependencies>
```

**3、编辑yaml文件**

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/jpa?characterEncoding=utf8&useSSL=false
    username: root
    password: 3155530
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
server:
  port: 8088
```

**4、启动项加注解**



```java
@SpringBootApplication
@EntityScan(basePackages = {"com.liran.springdatajpatest.pojo"})
@EnableJpaRepositories(basePackages = {"com.liran.springdatajpatest.repositories"})
```



**5、实体类**

@Entity  //将实体类指定为jpa实体

```java
package com.liran.springdatajpatest.pojo;

import lombok.Data;

import javax.persistence.*;

/**
 * @author obt
 */
@Data
@Entity  //将实体类指定为jpa实体
@Table(name = "user")
public class User {
    public User(){
    }
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Integer id;

    @Column(nullable = true)
    private String username;
    @Column(nullable = true)
    private String birthday;
    @Column(nullable = true)
    private String sex;
    @Column(nullable = true)
    private String address;
}
```



## 遇到的坑

**1、启动项没有加注解导致数据库无法自动建表**

```java
@SpringBootApplication
@EntityScan(basePackages = {"com.liran.springdatajpatest.pojo"})
@EnableJpaRepositories(basePackages = {"com.liran.springdatajpatest.repositories"})
```

**2、post请求在api工具里用get提交**



**3、controller内加入了`@RequestBody`**

`@RequestBody`是接受json格式的数据

`url?username=xxx`接受数据的形式可以直接用实体类直接接收

------



# 二、SpingData Repositories

**1、CrudRepository**

```java
@NoRepositoryBean
public interface CrudRepository<T, ID extends Serializable> extends Repository<T, ID> {
	<S extends T> S save(S entity);//保存
	<S extends T> Iterable<S> save(Iterable<S> entities);//批量保存
	T findOne(ID id);//根据id查询一个对象
	boolean exists(ID id);//判断对象是否存在
	Iterable<T> findAll();//查询所有的对象
	Iterable<T> findAll(Iterable<ID> ids);//根据id列表查询所有的对象
	long count();//计算对象的总个数
	void delete(ID id);//根据id删除
	void delete(T entity);//删除对象
	void delete(Iterable<? extends T> entities);//批量删除
	void deleteAll();//删除所有
}
```

**2、分页排序**

要进行分页排序查询，只需要创建接口继承`PagingAndSortingRepository`即可

```java
@Repository
public interface Test3Rpository extends PagingAndSortingRepository<User, String> {
}
```

使用

```java
@GetMapping("/getAllPage")
public List<Object> getAllPage(){
    Sort sort = Sort.by("id").descending();
    Page<User> all = repository3.findAll(PageRequest.of(0, 4,sort));
    List<Object> page = new ArrayList<>();
    page.add(all.getTotalPages());
    page.add(all.getTotalElements());
    page.add(all.getContent());

    return page;
}
```

# 三、自定义查询

**1、jpql**

①@Query

​	参数设置：

						1. 索引： ?数字

         ```java
         @Query("from User where username = ?1")
         List<User> findUserByUsername(String username);
         ```

						2. 具名： :参数名   结合`@Param`注解

         ```java
         @Query("from User where username = :username")
         List<User> findUserByUsername(@Param("username") String username);
         ```

②增删改：必须加上`@Tranctional`和`@Modifying`
