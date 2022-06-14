---
title: 原型链、Object、Function
date: 2022-06-14 10:40:16
tags: 笔记
---

有如下代码：

```javascript
//代码1
function People(){}
var p = new People()
p.__proto__ === People.prototype 
People.__proto__ === Function.prototype
People.prototype.__proto__ === Object.prototype
 
//代码2
Function.prototype === Function.__proto__         
Function.prototype === Object.__proto__           
Function.prototype.__proto__ === Object.prototype 
Function instanceof Object
 
//代码3
Object instanceof Function
Function instanceof Object
Function instanceof Function
Object instanceof Object
```

- 问题1：画出代码1的原型图？
- 问题2：从代码2你能得出什么结论？试画出原型图？
- 问题3：解释代码3的原因？

\-------------------------------------------------------------------------

**解答：**

在解答上面的问题之前，先记住下面几句话，这几句话能解释一切关于原型方面的问题：

1. **当 new 一个函数的时候会创建一个对象，『函数.prototype』 等于 『被创建对象.\_\_proto\_\_』**
2. **一切函数都是由 Function 这个函数创建的，所以『Function.prototype === 被创建的函数.\_\_proto\_\_』**
3. **一切函数的原型对象都是由 Object 这个函数创建的，所以『Object.prototype === 一切函数.prototype.\_\_proto\_\_』**

下面是代码1的原型图：

![](%E5%8E%9F%E5%9E%8B%E9%93%BE%E3%80%81Object%E3%80%81Function/format,png.png)

- （1）People函数创建了对象 p，所以People.prototype === p.\_\_proto\_\_；
- （2）Object函数创建了People.prototype对象，所以Object.prototype === People.prototype.\_\_proto\_\_；
- （3）People 作为对象的角色被函数Function创建，所以 Function.prototype === People.\_\_proto\_\_

下面是代码2的原型图：

![](%E5%8E%9F%E5%9E%8B%E9%93%BE%E3%80%81Object%E3%80%81Function/format,png-16551744278251.png)

- （1）任何函数都是 Function 创建，所以Function 创建了 Function，所以 Function.prototype === Function.\_\_proto\_\_；
- （2）Object 也是函数。所以Function创建了Object，所以 Function.prototype === Object.\_\_proto\_\_ ；
- （3）Function.prototype 是普通对象，普通对象是由Object创建的，所以 Function.prototype.\_\_proto\_\_ === Object.prototype

关于代码3：

instanceof 的作用是判断一个对象是不是一个函数的实例。比如 obj instanceof fn， 实际上是判断fn的prototype是不是在obj的[原型链](https://so.csdn.net/so/search?q=%E5%8E%9F%E5%9E%8B%E9%93%BE&spm=1001.2101.3001.7020)上。比如: obj.\_\_proto\_\_ === fn.prototype， obj.\_\_proto\_\_.\_\_proto\_\_ === fn.prototype，obj.\_\_proto\_\_...\_proto\_\_ === fn.prototype，只要一个成立即可。

所以（根据图2来找）

- 对于 Function instanceof Function，因为 Function.\_\_proto\_\_ === Function.prototype，所以为true。
- 对于 Object instanceof Object， 因为 Object.\_\_proto\_\_.\_\_proto\_\_ === Function.prototype.\_\_proto\_\_ === Object.prototype ， 所以为true
- 对于 Function instanceof Object, 因为 Function.\_\_proto\_\_.\_\_proto\_\_ === Function.prototype.\_\_proto\_\_ === Object.prototype， 所以为 true
- 对于 Object instanceof Function， 因为 Object.\_\_proto\_\_ === Function.prototype，所以为 true

至此，问题全被完美解决。
