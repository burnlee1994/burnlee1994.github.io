---
title: Java8新特性
date: 2022-02-22 00:17:16
tags: 笔记
---

# lambda

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

```java
// 筛选出年龄大于20的人
//  测试匿名内部类
    @Test
    public void test3() {

        List<Employee> list = filterEmployee(this.list, new MyPredicate<Employee>() {
            @Override
            public boolean test(Employee employee) {
                return employee.getAge() > 20;
            }
        });
        for (Employee employee : list) {
            System.out.println(employee);
        }
    }
```

匿名内部类简化了很多类的编写，但是new后面括号一系列的代码依旧很繁琐，可以继续简化

------

**3、lambda表达式**

匿名内部类中

![image-20220222235131499](Java8%E6%96%B0%E7%89%B9%E6%80%A7/image-20220222235131499.png)

有用的关键代码只有框中那一句，则可用lambda表达式简化

```java
// 测试lambda表达式
@Test
public void test4() {
    List<Employee> list = filterEmployee(this.list, (e) -> e.getAge() > 20);
    for (Employee employee : list) {
        System.out.println(employee);
    }
}
```

------

**4、流Stream api**

终极简化之JAVA8流

```java
@Test
public void test5(){
    list.stream().filter(e->e.getAge()>20).forEach(System.out::println);
}
```



# 练习

![image-20220223010018539](Java8%E6%96%B0%E7%89%B9%E6%80%A7/image-20220223010018539.png)

  

```java
// 1、
public void test5() {
    Collections.sort(list, (x, y) -> {
        // 如果年龄相同按姓名比
        if (x.getAge().equals(y.getAge())) {
            return x.getName().compareTo(y.getName());
        } else {
            return x.getAge().compareTo(y.getAge());
        }
    });
    for (Employee employee : list) {
        System.out.println(employee);
    }
```

# stream Api

**stream操作的三个步骤：**

1、创建stream

一个数据源（如：集合、数组），获取一个流

2、中间操作

一个中间操作链，对数据源的数据进行处理

3、终止操作

一个终止操作，执行中间操作链，并产生结果

![stream流过程](Java8%E6%96%B0%E7%89%B9%E6%80%A7/image-20220224005635245.png)

------

**一、创建流**

①通过`Collection `系列集合提供的`stream()`或`parallelStream()`

```java
List<String> list = new ArrayList<>();
Stream<String> stream = list.stream();
```

②通过`Arrays`类中的`stream()`方法，创建数组流

```java
Integer[] array = new Integer[10];
Stream<Integer> steam = Arrays.stream(array);	
```

③通过`Stream`类中的`of()`方法

```java
Stream<String> stream = stream.of("aa","bb","cc");
```

④创建无限流

```
// 迭代
```

------

**二、中间操作**

1、筛选

①filter

②limit

③skit

④distinct：筛选，通过流生成元素的hashCode()和equals()去除重复元素

2、映射

①map：接收lambda，将元素转换成其他形式或提取信息，接收一个函数作为参数，该函数会被应用到每个元素上，并将其映射成一个新的元素。

```java
// 把字符串改写成大写再输出

// 提取employee中的名字
```



②flatMap：接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有流链接成一个流



3、排序

①sorted：自然排序

②sorted(Comparator com)：定制排序

