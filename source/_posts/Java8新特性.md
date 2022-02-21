---
title: Java8新特性
date: 2022-02-22 00:17:16
tags: 笔记
---

# 用途

举例：

有一个`employee`实例，拥有字段

```java
String name;
Integer age;
Integer salary;
```

假如这里有3个人，信息对应字段

①张三，18，4000；

②李四，25，3000；

③王五，35，5999；

我们要删选出年龄大于20岁的人。

在JAVA8之前，我们可以使用两种方法：

**1、策略设计模式**

定义一个接口`MyPredicate`

```java
public interface MyPredict<T> {
    public boolean test(T t);
}
	
```

测试代码中申明方法，这个方法的参数有一个List`employees`（此时只是虚拟)，`MyPredicate mp`。

```java
public List<Employee> filterEmployee(List<Employee> employees, MyPredicate<Employee> mp) {
    List<Employee> list = new ArrayList<>();

    for (Employee employee : employees) {
        if (mp.test(employee)) {
            list.add(employee);
        }
    }
    return list;
}
```

此方法中调用了接口mp中的方法test，但此时test并未写死，这里就是策略设计模式的重点，再写一个类实现`MyPredicate`。

```java
public class FilterByAge implements MyPredicate<Employee> {
    @Override
    public boolean test(Employee employee) {
        return employee.age > 20;
    }
}
```

在类中写明筛选条件`age > 20`，这样就可以避免写很多很多类。

------

**2、匿名内部类**

上面的方式虽然减少了很多繁琐的类，但仍然要写有些类去实现接口，这里可以使用匿名内部类继续简化。
